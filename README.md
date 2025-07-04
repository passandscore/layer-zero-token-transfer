# ArtToken LayerZero OFT Transfer

A web-based interface for transferring ArtToken across different blockchain networks using LayerZero's Omnichain Fungible Token (OFT) protocol.

## Overview

This application enables users to transfer ArtToken between supported networks seamlessly. It provides a user-friendly interface for cross-chain token transfers with real-time fee estimation and transaction parameter visibility.

## Features

- **Cross-Chain Transfers**: Transfer ArtToken between Base and Linea networks
- **Network Mode Toggle**: Switch between testnet and mainnet environments
- **Real-Time Fee Estimation**: Automatic calculation of LayerZero messaging fees
- **Transaction Parameter Visibility**: View all transaction parameters before execution
- **Wallet Integration**: MetaMask wallet connection and management
- **Responsive Design**: Works on desktop and mobile devices

## Supported Networks

### Testnet
- **Base Testnet** (Chain ID: 84532)
  - Contract: `0x076cAaF62A40B8f8bB12c118cB8C346a1c9B0f2f`
  - LayerZero EID: 40287

- **Linea Testnet** (Chain ID: 59141)
  - Contract: `0x076cAaF62A40B8f8bB12c118cB8C346a1c9B0f2f`
  - LayerZero EID: 40245

### Mainnet
- **Base Mainnet** (Chain ID: 8453)
  - Contract: `0x4DEC3139f4A6c638E26452d32181fe87A7530805`
  - LayerZero EID: 30184

- **BNB Chain Mainnet** (Chain ID: 56)
  - Contract: `0x4DEC3139f4A6c638E26452d32181fe87A7530805`
  - LayerZero EID: 30102

## How to Use

### Prerequisites
1. **MetaMask Wallet**: Install and set up MetaMask browser extension
2. **Network Configuration**: Add supported networks to MetaMask
3. **Token Balance**: Ensure you have ArtToken on the source network

### Step-by-Step Guide

1. **Connect Wallet**
   - Click "Connect Wallet" button
   - Approve MetaMask connection request
   - Verify your wallet address is displayed

2. **Select Network Mode**
   - Choose between "Testnet" or "Mainnet"
   - The interface will automatically detect your current network
   - Available destination chains will update accordingly

3. **Enter Transfer Details**
   - **Recipient Address**: Enter the destination wallet address
   - **Amount**: Specify the amount of ArtToken to transfer
   - **Destination Chain**: Select the target network (automatically filtered to exclude current network)

4. **Review Transaction**
   - Click "Show Transaction Parameters" to view all contract call details
   - Review the estimated fees and parameters
   - Close the modal when ready to proceed

5. **Execute Transfer**
   - Click "Send" button
   - Approve the transaction in MetaMask
   - Wait for confirmation on both source and destination networks

### Transaction Flow

1. **Fee Estimation**: The app calls `quoteSend()` to estimate LayerZero messaging fees
2. **Options Retrieval**: Gets enforced options from the contract using `enforcedOptions()`
3. **Parameter Construction**: Builds the send parameters with destination EID, recipient, amount, and options
4. **Transaction Execution**: Calls the `send()` function with calculated fees
5. **Cross-Chain Processing**: LayerZero handles the cross-chain messaging and token transfer

## Technical Details

### Contract Functions Used

```solidity
// Get token balance
function balanceOf(address owner) view returns (uint256)

// Execute cross-chain transfer
function send(
    (uint32, bytes32, uint256, uint256, bytes, bytes, bytes) sendParam,
    (uint256, uint256) messagingFee,
    address refundAddress
) payable

// Estimate transfer fees
function quoteSend(
    (uint32, bytes32, uint256, uint256, bytes, bytes, bytes) sendParam,
    bool payInLzToken
) view returns (uint256 nativeFee, uint256 lzTokenFee)

// Get enforced options for destination
function enforcedOptions(uint32 eid, uint16 msgType) view returns (bytes)
```

### Send Parameters Structure

```javascript
const sendParam = [
    parseInt(destinationChainId), // dstEid - LayerZero endpoint ID
    recipientEncoded,             // to - Recipient address (32-byte padded)
    amountWei,                    // amountLD - Amount in wei
    amountWei,                    // minAmountLD - Minimum amount to receive
    options,                      // extraOptions - Enforced options from contract
    "0x",                         // composeMsg - Empty for basic transfer
    "0x"                          // oftCmd - Empty for basic transfer
];
```

### Fee Structure

- **Native Fee**: ETH/BNB required for gas and LayerZero messaging
- **LZ Token Fee**: Optional fee paid in LZ tokens (set to 0 in this implementation)

## Error Handling

The application includes comprehensive error handling for:

- **Network Mismatch**: Alerts when user is on unsupported network
- **Invalid Address**: Validates recipient address format
- **Insufficient Balance**: Checks token balance before transfer
- **Contract Errors**: Handles LayerZero contract call failures
- **Transaction Failures**: Provides feedback on failed transactions

## Security Considerations

- **Address Validation**: All recipient addresses are validated using ethers.js
- **Amount Validation**: Ensures positive, valid amounts
- **Network Verification**: Confirms user is on supported network before allowing transfers
- **Fee Estimation**: Always estimates fees before transaction execution
- **Parameter Visibility**: Users can review all transaction parameters before execution

## Browser Compatibility

- **Chrome**: Full support with MetaMask
- **Firefox**: Full support with MetaMask
- **Safari**: Full support with MetaMask
- **Edge**: Full support with MetaMask

## Dependencies

- **ethers.js v5.7.2**: Ethereum library for wallet interaction and contract calls
- **MetaMask**: Browser wallet for transaction signing
- **LayerZero Protocol**: Cross-chain messaging infrastructure

## Development

### Local Setup

1. Clone the repository
2. Open `index.html` in a web browser
3. Ensure MetaMask is installed and configured
4. Add supported networks to MetaMask

### Network Configuration

Add these networks to MetaMask:

**Base Testnet**
- Network Name: Base Testnet
- RPC URL: https://goerli.base.org
- Chain ID: 84532
- Currency Symbol: ETH

**Linea Testnet**
- Network Name: Linea Testnet
- RPC URL: https://rpc.goerli.linea.build
- Chain ID: 59141
- Currency Symbol: ETH

**Base Mainnet**
- Network Name: Base
- RPC URL: https://mainnet.base.org
- Chain ID: 8453
- Currency Symbol: ETH

**BNB Chain Mainnet**
- Network Name: BNB Smart Chain
- RPC URL: https://bsc-dataseed.binance.org
- Chain ID: 56
- Currency Symbol: BNB

## Troubleshooting

### Common Issues

1. **"Unsupported network" Error**
   - Ensure you're connected to a supported network
   - Switch to Base or Linea testnet/mainnet

2. **"quoteSend failed" Error**
   - Check if you have sufficient token balance
   - Verify recipient address is correct
   - Ensure you have enough native tokens for fees

3. **Transaction Stuck**
   - Check LayerZeroScan for transaction status
   - Verify gas settings in MetaMask
   - Ensure sufficient native token balance for gas

4. **Destination Chain Not Available**
   - The app automatically filters out your current network
   - Switch to a different network to see available destinations

### Support

For technical support or questions about ArtToken transfers:
- Check LayerZero documentation: https://layerzero.network/
- Review transaction status on LayerZeroScan: https://layerzeroscan.com/
- Verify network status and contract addresses

## License

This project is provided as-is for ArtToken cross-chain transfer functionality. 