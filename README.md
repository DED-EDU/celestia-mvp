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
I won't go into too much detail here but basically we are converting the block into binary, adding that to a bytes.NewReader function, in order to get a type that implements ```io.Reader``` from a ```[]byte``` slice.

Finally we use our client to PutCar(), passing in the context and the reformatted block. CAR in this case refers to Content Archive. In short:

The Content Archive format is a way of packaging up content addressed data into archive files that can be easily stored and transferred. You can think of them like TAR files that are designed for storing collections of content addressed data.

The type of data stored in CARs is defined by Interplanetary Linked Data (IPLD). IPLD is a specification and set of implementations for structured data types that can link to each other using a hash-based Content Identifier (CID). Data linked in this way forms a Directed Acyclic Graph, or DAG. (@dev: this bit may be out of scope and subject to removal)


### Links

* [rollmint](https://github.com/DED-EDU/rollmint)

* [cosmos-sdk-rollmint](https://github.com/DED-EDU/cosmos-sdk-rollmint)

* [gm](https://github.com/DED-EDU/gm)

Note: the repositories above contain distinct differences from celestiaorg repos
