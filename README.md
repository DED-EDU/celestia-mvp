# celestia-mvp

Long-term Data Storage with Celestia Rollups

## Instructions

First, create a Web3.Storage account [here](https://web3.storage/login/) or using the [cli](https://github.com/web3-storage/w3up-cli) (Keep this safe!).

Next, fork [Rollmint](https://github.com/celestiaorg/rollmint). Navigate to ```block/manager.go```. We will need to add [go-w3s-client](https://github.com/web3-storage/go-w3s-client) to the imports and install that package. We will start writing code at line ~449 (after the block is applied)...

As follows is the code added to that file: 

```
c, err := w3s.NewClient(w3s.WithToken("your_API_token_here"))
	dataBytes, err := block.Data.MarshalBinary()
	if err != nil {
		return err
	}

	r := bytes.NewReader(dataBytes)
	cid, err := c.PutCar(ctx, r)
	if err != nil {
		return err
	}

	fmt.Println(cid)
  ```

### Links

* [rollmint](https://github.com/DED-EDU/rollmint)

* [cosmos-sdk-rollmint](https://github.com/DED-EDU/cosmos-sdk-rollmint)

* [gm](https://github.com/DED-EDU/gm)

Note: the repositories above contain distinct differences from celestiaorg repos
