# Deploy Your Daml Project on Canton LocalNet

> **Who this is for?** 

You showed up at a hackathon (or a bootcamp). You've written some Daml, built your `.dar`, and now you're staring at the Canton docs wondering where to even begin. This guide is for you.

> **What you'll walk away with?** 

Your Daml contracts running on a Canton LocalNet with real wallets, Canton Coin (test), and multi-party transactions. No DevNet whitelisting. No waiting 10 days. No Cloud Costs to Start Building.

> **Platform:** macOS and Linux only. Windows users can use WSL 2 for the same.

## Before We Start: What Even Is This Thing?

Let's cut through the jargon real quick.

- **Canton LocalNet** is a full Canton Local Network running in Docker. Not a simulation. Not a trimmed down sandbox. An actual Canton Network with validators, a synchronizer, wallets, Canton Coin. It's built into the `cn-quickstart` repository from Digital Asset and it's what you're going to use.

- **cn-quickstart** is the GitHub repo that gives you LocalNet plus a reference licensing app built on top of it. You're going to clone it, gut the reference app parts you don't need, and we gonna plug your Daml project in instead.

Here's the mental model of three roles you need to understand:

| What | What It Actually Is | Why You Care |
|------|---------------------|--------------|
| **Synchronizer** | sequences and orders transactions | It's running in LocalNet already. |
| **Validator** | Runs a participant node + wallet for one. | LocalNet gives you 3 Participants |
| **Party** | A named identity on the ledger (like an address) | Your Daml contracts are between parties |

LocalNet gives you three validators: `app-provider`, `app-user`, and `sv` (Super Validator). Think of `app-provider` as your app/company and `app-user` as a customer. The `sv` runs the infrastructure. For most hackathon projects, you'll deploy your DAR to `app-provider` and have parties on both `app-provider` and `app-user` interact with your contracts as a multi-party workflow.

**What about Keycloak?** 

Keycloak is the authentication/OAuth2 layer in the stack. **Enable it during setup** (`make setup` will ask). It's required if you want to use the wallet UI and do Canton Coin transfers. Without it things get complicated. Just say **yes** and move on.

## Prerequisites Checklist

Before you do anything else, check these off:

- [ ] **Docker Desktop** installed and running (`docker info` returns something, not an error)
- [ ] Docker Desktop has **at least 8 GB of memory** allocated (Docker Desktop > Settings > Resources > Memory)
- [ ] **Git** installed
- [ ] **`dpm`** installed and in your PATH, you need this because you're building your Project.
- [ ] **Your Daml project** should have a `daml.yaml` and your `.daml` source files ready.
- [ ] Docker Hub account (free) you need to be logged in `docker login`

> **Check your dpm PATH:** 

Run `dpm version`. If it errors, your dpm bin isn't in PATH.

> **Memory is not optional.** 

If Docker has less than 8 GB, containers will crash mid startup and you'll waste an hour debugging what is actually just an OOM kill. Allocate the memory first.

## Step 1: Clone cn-quickstart

```bash
git clone https://github.com/digital-asset/cn-quickstart.git
cd cn-quickstart
```

Now go into the quickstart directory, **this is where you'll live for the rest of the guide:**

```bash
cd quickstart
```

> Almost everything from here on runs from inside `quickstart/`. The `Makefile` is here. If a `make` command errors with "no Makefile found", you're in the wrong directory.

## Step 2: Configure LocalNet

Run the setup wizard:

```bash
make setup
```

You'll get a few prompts. Here's exactly what to answer:

| Prompt | Answer |
|--------|--------|
| Enable Observability? | **n** |
| Enable OAuth2? | **y** |
| Party hint | **just press Enter** |
| Enable TEST MODE? | **n** |

When it finishes, it writes a `.env.local` file. Done. You can re run `make setup` anytime to change these, IF Needed.

## Step 3: Add Your Daml Project to the Quickstart

This is the part nobody explains. Here's how to get your contracts into LocalNet.

### 3a. Where to put your Daml files

The quickstart's Daml code lives in `quickstart/daml/licensing/`. That's the reference licensing app. You have two options:

**Option A: Add alongside the existing app (recommended for hackathons)**

Create a new directory for your project inside `daml/`:

```
quickstart/
  daml/
    licensing/          ← the existing reference app, leave this alone
    your-project/       ← create this
      daml/
        YourModule.daml
      daml.yaml
```

This is cleaner. You keep the existing reference app intact (useful for reference), and add your own as a separate package.

**Option B: Replace the existing app**

If you want to start fresh, delete the contents of `daml/licensing/` and drop your project files there. Update the paths accordingly in the steps below. Only do this if you're confident you won't need the reference app for anything.

### 3b. Set up your `daml.yaml`

Your project needs a `daml.yaml`. If you built it with `dpm`, you already have one. Make sure the SDK version in your `daml.yaml` matches what the quickstart uses.

Check the quickstart's SDK version:

```bash
cat .env | grep DAML_SDK_VERSION
```

Now check your `daml.yaml`:
```yaml
sdk-version: 3.x.x   # this should match what you just found
```

If they don't match, update your `daml.yaml` to use the quickstart's SDK version.

### 3c. Register your package in `multi-package.yaml`

The quickstart uses a `multi-package.yaml` to build all Daml packages together. Open `quickstart/daml/multi-package.yaml` and add your project:

```yaml
packages:
  - licensing       # existing reference app
  - your-project    # add this line (relative to the daml/ directory)
```

### 3d. Build everything

```bash
make build
```

This compiles all Daml packages (including yours) into `.dar` files, generates Java bindings for the backend, and builds the frontend. Your compiled DAR will end up at:

```
quickstart/daml/your-project/.daml/dist/your-project-<version>.dar
```

> **Build errors?** 99% of the time it's an SDK version mismatch or a missing dependency in your `daml.yaml`. Double-check both. If your project depends on Splice DARs (Canton Coin interfaces etc.), check `daml/dars/` the quickstart bundles them there.

## Step 4: Start LocalNet

In a second terminal, start log collection (keep this running the whole time):

```bash
cd quickstart
make capture-logs
```

Back in your first terminal:

```bash
make start
```

This spins up the whole Canton Network locally. It takes ~5 minutes the first time. You'll see Docker pulling images and containers starting. When it settles and you stop seeing a flood of log output, you're up.

## Step 5: Upload Your DAR to LocalNet

LocalNet is running, but your contracts aren't on it yet. You need to upload your DAR to the participant nodes that will use it.

### 5a. Get an admin token

The JSON API requires an auth token. Grab the admin token for the App Provider:

```bash
export PROVIDER_ADMIN_TOKEN=$(curl -fsS \
  "http://keycloak.localhost:8082/realms/AppProvider/protocol/openid-connect/token" \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'client_id=app-provider-validator' \
  -d 'client_secret=6m12QyyGl81d9nABWQXMycZdXho6ejEX' \
  -d 'grant_type=client_credentials' \
  -d 'scope=openid' | jq -r .access_token)

echo $PROVIDER_ADMIN_TOKEN   # should print a long JWT string, not empty
```

> **Token expired?** Tokens expire after a while. Just re run this command to get a fresh one. If you get `Cannot iterate over null` from jq, Keycloak might still be starting, wait a minute and retry.

### 5b. Upload your DAR

```bash
# Replace the path with your actual DAR file path

curl -X POST http://localhost:3975/v2/packages \
  -H "Authorization: Bearer $PROVIDER_ADMIN_TOKEN" \
  -H "Content-Type: application/octet-stream" \
  --data-binary @./daml/your-project/.daml/dist/your-project-0.0.1.dar
```

A `{}` response means success. Your contracts are now deployed on the App Provider's participant node.

**IF your contracts involve multiple parties across both the App Provider and App User validators, upload to the App User node too:**

```bash
export USER_ADMIN_TOKEN=$(curl -fsS \
  "http://keycloak.localhost:8082/realms/AppUser/protocol/openid-connect/token" \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'client_id=app-user-validator' \
  -d 'client_secret=6m12QyyGl81d9nABWQXMycZdXho6ejEX' \
  -d 'grant_type=client_credentials' \
  -d 'scope=openid' | jq -r .access_token)

curl -X POST http://localhost:2975/v2/packages \
  -H "Authorization: Bearer $USER_ADMIN_TOKEN" \
  -H "Content-Type: application/octet-stream" \
  --data-binary @./daml/your-project/.daml/dist/your-project-0.0.1.dar
```

### 5c. Get your package ID

You'll need the package ID to create contracts via the API:

```bash
dpm damlc inspect-dar ./daml/your-project/.daml/dist/your-project-0.0.1.dar
```

Find the line with your project name and no `-dalf` extension. It looks like:
```
your-project-0.0.1-<64-char-hex-string>
```

That 64-character hex string is your package ID. Save it:

```bash
export PACKAGE_ID=$(dpm damlc inspect-dar ./daml/your-project/.daml/dist/your-project-0.0.1.dar \
  | grep "your-project-0.0.1-" | grep -v "dalf" | tail -1 | awk '{print $2}' | tr -d '"')
echo $PACKAGE_ID
```

## Step 6: Discover Your Party IDs

Parties in Canton have long identifier strings (not just names). You need these to create contracts. Grab them:

```bash
# App Provider party
APP_PROVIDER_PARTY=$(curl -s -H "Authorization: Bearer $PROVIDER_ADMIN_TOKEN" \
  http://localhost:3975/v2/parties | \
  jq -r '.partyDetails[] | select(.party | startswith("app_provider_quickstart-")) | .party')
echo "Provider: $APP_PROVIDER_PARTY"
```

```bash
# App User party
APP_USER_PARTY=$(curl -s -H "Authorization: Bearer $USER_ADMIN_TOKEN" \
  http://localhost:2975/v2/parties | \
  jq -r '.partyDetails[] | select(.party | startswith("app_user_quickstart-")) | .party')
echo "User: $APP_USER_PARTY"
```

You'll get back something like `app_provider_quickstart-1::122045abc...`. That full string is the party identifier. Use it everywhere you'd use a party in API calls.

## Step 7: Create a Contract

Now the fun part lol...Let's create a contract on LocalNet via the JSON Ledger API.

This example assumes you have a template like:

```daml
template MyContract
  with
    provider : Party
    user : Party
    someData : Text
  where
    signatory provider
    observer user
```

**Create it:**

```bash
curl -X POST http://localhost:3975/v2/commands/submit-and-wait \
  -H "Authorization: Bearer $PROVIDER_ADMIN_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{
    \"commands\": [{
      \"CreateCommand\": {
        \"template_id\": \"${PACKAGE_ID}:YourModule:MyContract\",
        \"create_arguments\": {
          \"provider\": \"$APP_PROVIDER_PARTY\",
          \"user\": \"$APP_USER_PARTY\",
          \"someData\": \"hello from hackathon\"
        }
      }
    }],
    \"act_as\": [\"$APP_PROVIDER_PARTY\"],
    \"read_as\": [\"$APP_PROVIDER_PARTY\"]
  }"
```

A response containing a `transaction_id` means your contract was created. It's live on LocalNet YAYAY 🎉

> **Template ID format:** `<packageId>:<ModuleName>:<TemplateName>` note the colons, not dots.

## Step 8: Tap Some Canton Coin and Test Transfers

LocalNet has full wallet functionality. On LocalNet, CC is available automatically via Wallet UI Tap option.

### Open the wallet UIs

- App User Wallet: [http://wallet.localhost:2000](http://wallet.localhost:2000)
- App Provider Wallet: [http://wallet.localhost:3000](http://wallet.localhost:3000)

Log in with:
- **App User:** username `app-user`, password `abc123`
- **App Provider:** username `app-provider`, password `abc123`
- OR If the passwords show invalid, login into main admin Keycloak via username `admim`, password `admin` and make a new password via users tab.

### Tap CC (App User)

In the App User wallet, find the **"Tap"** button. Add Some amount there(which is in USD), Say 500. It'll show up in the balance almost immediately converted into CC.

### Send CC between parties

In the App User wallet:
1. Click **Send**
2. Paste the App Provider's party ID (from Step 6)
3. Enter an amount
4. Confirm

You'll see the transfer show up in both wallets. This is real Canton Coin mechanics of privacy preserving transfers between parties on separate validators, mediated by the local synchronizer.

## Step 9: Interact via Daml Shell

The Daml Shell is a REPL for your LocalNet, great for exercising choices interactively without writing curl commands.

```bash
make shell
```

You'll drop into an interactive shell connected to the App Provider's participant. You can query active contracts, exercise choices, and inspect ledger state.

```
daml> import DA.List
daml> -- your Daml expressions here
```

## Resetting and Starting Clean

Things will break. That's fine. Here's how to wipe and start fresh:

```bash
make stop           # stop all containers
make clean-all      # remove data, volumes, everything
make build          # rebuild
make start          # start fresh
```

> `make clean-all` nukes all localnet ledger data, party registrations, CC balances, everything. It's a full reset. Do this whenever you hit a weird state you can't debug.

## Port Reference

Bookmark this. You'll refer to it constantly:

| Service | URL / Port | What It Is |
|---------|-----------|------------|
| App User JSON API | `localhost:2975` | Submit commands, query as App User |
| App Provider JSON API | `localhost:3975` | Submit commands, query as App Provider |
| SV JSON API | `localhost:4975` | Super Validator (rarely needed directly) |
| App User Ledger API (gRPC) | `localhost:2901` | Lower level gRPC access |
| App Provider Ledger API (gRPC) | `localhost:3901` | Lower level gRPC access |
| App User Wallet UI | `wallet.localhost:2000` | Wallet for App User |
| App Provider Wallet UI | `wallet.localhost:3000` | Wallet for App Provider |
| Scan UI | `scan.localhost:4000` | Network transaction explorer |
| Keycloak | `keycloak.localhost:8082` | Auth server |

## Common Issues

- **"Container failed to start"**

Almost always a memory issue. Check Docker Desktop memory allocation. 8 GB minimum, 12 GB if you can spare it.

- **"401 Unauthorized" on API calls**

Your token expired. Re run the token export commands from Step 5a.

- **"Empty reply from server" on DAR upload**

Network issue or the participant isn't ready yet. Wait 30 seconds and retry.

- **"409 Conflict" on DAR upload**

You already uploaded that DAR. That's fine it's idempotent, the package is already there.

- **Wallet UI shows blank / can't log in**

Keycloak might still be starting. Wait a minute and refresh. If it persists `make stop && make clean-all && make start`.

- **`make build` fails with "SDK version mismatch"**

Your `daml.yaml` SDK version doesn't match the quickstart's. Update your `daml.yaml` to match `DAML_SDK_VERSION` in the quickstart's `.env` file.

## Quick Reference: The Commands You'll Use Most

```bash
make install-daml-sdk    # Install the pinned Daml SDK
make setup               # Configure LocalNet
make build               # Compile everything (Daml + backend + frontend)
make capture-logs        # Start log collection (run in separate terminal)
make start               # Start LocalNet
make stop                # Stop LocalNet
make clean-all           # Full reset
make canton-console      # Open Canton Console (admin REPL)
make shell               # Open Daml Shell (contract REPL)
make help                # See all available cmd options.
```

*Built for Devs. If something in this guide is wrong or outdated, Open an issue or Make a PR, Guide made by [Jatin](https://x.com/Jpandya26), Developer Relations Manager, Canton Foundation.*