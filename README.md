# iridius-polkadot-ne-hackathon
Iridius - decentralising Web3 frontends

While blockchains technologically are decentralized, the ways users interact with them is not. A key centralized weak point are the server-hosted DApp websites, and all mobile apps and browser extensions currently in use by Blockchain users. This requires everyday users to trust the developer of the particular app, which is against blockchain spirit.

Just last week, Convex's frontend was hijacked. A few months ago BadgerDAO lost $130M, and Cream Finance and PancakeSwap were compromised in March 2021. The server of polkadot.js.org/apps, the main way Polkadot users govern and secure the network, is also vulnerable to these attacks, together with the Polkadot.js browser extension. For example, an attacker can push an update that makes users unknowingly delegate to a malicious validator, which would compromise the security of the Polkadot chain. 

Iridius presents a consensus-driven way to certify DApp websites. A community of auditors would vote on which is the "true" version of website.xyz. Then, once the user visits that website, the Iridius browser extension will verify if that website matches the version stored on-chain. If it doesn't, the user knows that the website might have been modified by a malicious actor, so they should not trust it. 

The project's demo consists of a Moonbeam smart contract, Moonbeam The Graph data feeder, a browser extension to verify websites, and two websites - one which gets verified successfully, and another one which has been modified by a bad actor. 

Team Members: Zvezdin, git@zvezd.in


## Testing the Demo

```bash
git clone --recurse-submodules git@github.com:Zvezdin/iridius-polkadot-ne-hackathon.git
```

### Setting up Extension

First, setup the Iridius verifier browser extension. [Download the chrome build from here](./extension-build-chrome.zip) (or build from source from [this repo](https://github.com/Zvezdin/iridius-demo-extension)). Unzip, then go into Chrome's context menu -> Extensions -> Turn on Developer mode (top right) -> Load unpacked -> select the unzipped folder. 

The "Code Verify" extension should be installed. Pin it to your extensions bar so you can see its icon. 

### Visiting Example Websites

If you open [https://zvezdin.github.io/iridius-demo-website/](https://zvezdin.github.io/iridius-demo-website/), then verification should succeed and the extension icon should become a green tick to indicate that. **Note: Metamask, and other extensions can make the verification fail. Please disable them, go into Incognito, or use a new Browser profile and re-install the Iridius extension**

The true hash of that website is stored in our Moonbeam smart contract. 

If you open [https://zvezdin.github.io/iridius-demo-website-failing/](https://zvezdin.github.io/iridius-demo-website-failing/), then verification should fail. 

The hash of that website is stored in our Moonbeam smart contract, but a malicious actor who accessed the server has modified the javascript :O , and now verification fails :(

### Under the hood

The extension hashes all JavaScript in both example websites, and then connects to our [on-chain smart contract deployed here](https://moonbase.moonscan.io/address/0x1d9fbdc6f9959451e0f8d83d3f6182c816ee79c8) to read what is the "true" hash for that website URL. If it matches, verification succeeds, failure otherwise. 

The connection to the smart contract happens using our [The Graph data feeder, located here](https://github.com/Zvezdin/iridius-the-graph). We're using The Graph so that the connection to the smart contract is decentralised - otherwise there would be yet another server to trust! 