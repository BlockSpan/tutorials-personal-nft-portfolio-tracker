# PERSONAL NFT PORTFOLIO TRACKER


## HOW TO USE THE BLOCKSPAN API TO CREATE YOUR PERSONAL NFT PORTFOLIO TRACKER

Blockspan is a leading provider of NFT API services, enabling developers to easily interact with the world of non-fungible tokens (NFTs). NFTs represent ownership of a unique item or piece of content on the blockchain. A personal NFT portfolio tracker is an invaluable tool for monitoring the value and transactions of your NFTs.


## REQUIREMENTS:
- Node.js and npm installed on your system.
- Basic knowledge of React.js
- Blockspan API key


## STEP 1: SET UP YOU REACT APPLICATION

First, you'll need to set up your React application. If you already have a React application set up, you can skip this step.

`npx create-react-app personal-nft-portfolio-tracker` 
`cd personal-nft-portfolio-tracker`

This will create a new React application named `personal-nft-portfolio-tracker` and navigate into the new directory.


## STEP 2: INSTALL AXIOS

We'll be using Axios to send HTTP requests to the Blockspan API. Install it with the following command:

`npm install axios`


## STEP 3: CREATE YOUR REACT COMPONENT

Next, you'll need to create a React component that uses the Blockspan API to fetch portfolio data. Create a new file in the `src` directory called `NFTPortfolioTracker.js` and include the following code:

```
import React, { useState } from 'react';
import axios from 'axios';
import './App.css';

const NFTPortfolioTracker = () => {
  const [blockchain, setBlockchain] = useState('eth-main');
  const [nfts, setNfts] = useState(null);
  const [address, setAddress] = useState('');
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(false);

  const fetchNFTs = async () => {
    setLoading(true)
    const options = {
      method: 'GET',
      url: `https://api.blockspan.com/v1/nfts/owner/${address}?chain=${blockchain}&include_nft_details=true&cursor=7&page_size=20`,
      headers: { accept: 'application/json', 'X-API-KEY': 'YOUR_BLOCKSPAN_API_KEY' },
    };

    try {
      const response = await axios.request(options);
      setNfts(response.data.results);
      setError(null);
    } catch (error) {
      setNfts(null);
      error.response.status === 401 ?
        setError('Invalid blockspan API key!') :
        setError('Error: Verify that chain and wallet address are valid!');
    }
    setLoading(false)
  };

  const handleBlockchainChange = (event) => {
    setBlockchain(event.target.value);
  };

  const checkData = (data) => (data ? data : 'N/A');

  return (
    <div>
      <h1 className="title">NFT Portfolio Tracker</h1>
      <p className="message">
        Select a chain and input a wallet address below to see what NFTs that wallet owns on that chain.
      </p>
      <div className="inputContainer">
        <select name="blockchain" value={blockchain} onChange={handleBlockchainChange}>
          <option value="eth-main">eth-main</option>
          <option value="arbitrum-main">arbitrum-main</option>
          <option value="optimism-main">optimism-main</option>
          <option value="poly-main">poly-main</option>
          <option value="bsc-main">bsc-main</option>
          <option value="eth-goerli">eth-goerli</option>
        </select>
        <input type="text" placeholder="Wallet Address" onChange={(e) => setAddress(e.target.value)} />
        <button onClick={fetchNFTs}>View Portfolio</button>
      </div>
      {loading ? (
        <p className='message'>Loading...</p>
      ) : (
        error ? (
          <p className="errorMessage">{error}</p>
        ) : (
          nfts && (
            <table>
              <thead>
                <tr style={{ backgroundColor: '#f2f2f2' }}>
                  <th>Token Name</th>
                  <th>Id</th>
                  <th>Token Type</th>
                  <th>Contract Address</th>
                  <th>Image</th>
                </tr>
              </thead>
              <tbody>
                {nfts.map((nft, index) => (
                  <tr key={index} style={{ backgroundColor: index % 2 === 0 ? '#f2f2f2' : 'white' }}>
                    <td>{checkData(nft.nft_details.token_name)}</td>
                    <td>{checkData(nft.id)}</td>
                    <td>{checkData(nft.token_type)}</td>
                    <td>{checkData(nft.contract_address)}</td>
                    <td>
                      <div className="imageContainer">
                        {nft.nft_details.cached_images && nft.nft_details.cached_images.medium_500_500 ? (
                          <img 
                            className="image" 
                            src={nft.nft_details.cached_images.medium_500_500} 
                            alt={nft.nft_details.name}/>
                        ) : (
                          <div className='message'>
                            Image not available.
                          </div>
                        )}
                      </div>
                    </td>
                  </tr>
                ))}
              </tbody>
            </table>
          )
        )
      )}
    </div>
  );
};

export default NFTPortfolioTracker;

```

Remember to replace `YOUR_BLOCKSPAN_API_KEY` with your actual Blockspan API key.


## STEP 4: UPDATING THE STYLES WITHIN CSS FILE

To enhance the user interface in the browser, replace all code in the App.css file with the following:

```
.App {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  min-height: 100vh;
  overflow-y: auto;
}

.title {
  margin-top: 20px;
  margin-bottom: 0;
  text-align: center;
}

.errorMessage {
  text-align: center;
  color: red;
  font-weight: bold;
}

.message {
  text-align: center;
}

.image {
  display: flex;
  justify-content: center;
  align-items: center;
}

.inputContainer {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 20px;
}

.inputContainer input {
  padding: 10px;
  font-size: 1em;
  width: 200px;
}

.inputContainer button {
  padding: 10px;
  font-size: 1em;
  background-color: #007BFF;
  color: white;
  border: none;
  cursor: pointer;
}

.inputContainer button:hover {
  background-color: #0056b3;
}

.imageContainer {
  display: flex;
  justify-content: center;
  width: 100%; 
}

.imageContainer img {
  width: 100%; 
  max-width: 100px;
  height: auto; 
}
.nftData {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
}

.nftData .image {
  display: flex;
  justify-content: center;
  align-items: center;
}

.nftData h2 {
  margin: 10px 0;
}

.nftData p {
  font-size: 1.2em;
  font-weight: bold;
}

td {
  padding: 10px;
  text-align: left;
}

th {
  padding: 10px;
  text-align: left;
}

.tableContainer {
  display: flex;
  justify-content: center;
}

```


## STEP 5: INTEGRATING COMPONENTS IN THE APP

Finally, let's use the `NFTPortfolioTracker` component in our main `App` component.

Open App.js and modify it as follows:

```
import React from 'react';
import NFTPortfolioTracker from './NFTPortfolioTracker.js';

function App() {

  return (
    <div className="App">
      <NFTPortfolioTracker/>
    </div>
  );
}

export default App;
```

Now, start the app with the following command:

`npm start`

You should now see the following:

- A drop down menu to select a blockchain
- A text box for wallet address
- A view portfolio button

Input the wallet address you wish to see the portfolio of, and click the view portfolio button. You should then see a table with all the NFTs owned by that wallet.

That's it! You've built a personal NFT portfolio tracker using Blockspan API and ReactJS! This is a simple example, but with the power of Blockspan API, you can build even more comprehensive and interactive NFT explorers. Happy coding!
