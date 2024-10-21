# AltLayer MACH AVS Operator Guide

This guide will walk you through the process of setting up and registering your node to participate in AltLayer MACH AVS, applicable for both testnet and mainnet environments.

---

### **System Requirements**
- **CPU**: 4 cores
- **Memory**: 16GB RAM
- **Storage**: 50GB SSD
- **Operating System**: Ubuntu 22.04 LTS
- **Software**:
  - Docker v24+
  - EigenLayer CLI
  - Full Node with RPC service (or managed RPC services for target rollup)

### **Minimal Stake Requirements**
- **Mainnet**: 0 ETH
- **Testnet**: 1 ETH

### **Supported Token Strategy**
- Supports Beacon Chain Ether and all Liquid Staking Tokens (LSTs) compatible with EigenLayer.

---

### **Operator Setup**

#### **Step 1: Key Generation and Wallet Funding**
1. **Install EigenLayer CLI** following the official guide.
2. **Generate ECDSA and BLS key pairs** using:
   ```bash
   eigenlayer operator keys create --key-type ecdsa [keyname]
   eigenlayer operator keys create --key-type bls [keyname]
   ```
   > 💡 Backup the generated private keys. By default, they are stored in `~/.eigenlayer/operator_keys/`.

3. **Fund the ECDSA address** with at least 0.3 ETH for registration purposes.
4. **Submit your ECDSA address** via the [MACH AVS Interest Form](https://forms.gle/3A6xNWwRf9ip9CnN9).

#### **Step 2: Register on EigenLayer as an Operator**
1. **Create Configuration Files**:
   ```bash
   eigenlayer operator config create
   ```
   This will generate `operator.yaml` and `metadata.json`.

2. **Edit `metadata.json`** to include your details:
   ```json
   {
     "name": "Example Operator",
     "website": "https://example.com/",
     "description": "Example description",
     "logo": "https://example.com/logo.png",
     "twitter": "https://twitter.com/example"
   }
   ```
3. **Host `metadata.json` on a public URL** and update the `operator.yaml` file with the metadata URL.
4. **Register the Operator**:
   ```bash
   eigenlayer operator register operator.yaml
   ```
   > You should see `✅ Operator is registered successfully to EigenLayer` upon successful registration.

5. **Check Registration Status**:
   ```bash
   eigenlayer operator status operator.yaml
   ```

### **Joining MACH AVS**
1. **Express Interest** by filling out the [AltLayer MACH AVS Form](https://forms.gle/LmPABTNQcwJhw4UB7).

2. **Clone the MACH AVS Repository**:
   ```bash
   git clone https://github.com/alt-research/mach-avs-operator-setup.git
   ```
3. **Select Your Network Directory**:
   - For Testnet:
     ```bash
     cd mach-avs-operator-setup/holesky/mach-avs/altlayer-multi-mach
     ```
   - For Mainnet:
     ```bash
     cd mach-avs-operator-setup/ethereum/mach-avs/altlayer-multi-mach
     ```

4. **Opt-In to MACH AVS**:
   ```bash
   ./run.sh opt-in
   ```
   > Ensure your infrastructure meets the requirements and that you are whitelisted before opting in.

5. **Opt-Out** if Needed:
   ```bash
   ./run.sh opt-out
   ```

### **Running MACH AVS**

#### **Step 1: Create Environment Configuration**
1. Copy the example environment file:
   ```bash
   cp .env.example .env
   ```

2. **Edit `.env`** to configure the paths, keys, and other settings. Example:
   ```bash
   USER_HOME=${HOME}
   EIGENLAYER_HOME=${USER_HOME}/.eigenlayer
   OPERATOR_ECDSA_ADDRESS=<Your ECDSA Address>
   NODE_BLS_KEY_FILE_HOST=${EIGENLAYER_HOME}/operator_keys/opr.bls.key.json
   OPERATOR_BLS_KEY_PASSWORD=<Your BLS Password>
   ```

#### **Step 2: Configure Rollup Endpoint**
1. **Navigate to `/configs/chain`** and update the respective chain configs (e.g., Optimism, Arbitrum).
2. **Replace the `url` values** with your managed RPC service endpoints.

#### **Step 3: Launch MACH AVS Using Docker**
1. **Pull Docker Images**:
   ```bash
   docker compose pull
   ```
2. **Run MACH AVS**:
   ```bash
   docker compose up -d
   ```

#### **Optional: Running MACH AVS on Kubernetes**
You can use the Helm chart available in the MACH AVS Operator Setup repository to deploy within your Kubernetes cluster.

---

### **Verification and Troubleshooting**

#### **Check Running Containers**
```bash
docker compose ps
```
Example output:
```plaintext
NAME                         STATUS          PORTS
mach-avs-operator-node       Up              9092/tcp
mach-avs-operator-verifier   Up              9094/tcp
mach-avs-reverse-proxy-1     Up              9091/tcp
```

#### **View Logs for Block Validation**
```bash
docker logs <container_name>
```

Logs should indicate block validation, e.g.:
```plaintext
[2024-04-01T17:06:28Z INFO  actor::scheduler] Validate block #166853 passed
```

### **Frequently Asked Questions**
1. **How do I know if my operator is running?**
   - Check if your Docker container is up using `docker compose ps`.
   - Review logs to confirm active validation.
   
2. **What should I do if I encounter insufficient stake errors?**
   - Ensure your ECDSA address has the required minimal ETH balance.

3. **Will there be token rewards?**
   - Yes, AltLayer MACH AVS mainnet operators may receive future rewards in ALT tokens.

4. **How do I run an RPC node for various rollups?**
   - Follow the documentation provided [here](https://docs.optimism.io/builders/node-operators/overview#tutorials).
