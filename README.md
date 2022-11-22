# celestia-mvp

Long-term Data Storage with Celestia Rollups

## Prerequisites

* Complete the [Celestia gm world tutorial](https://docs.celestia.org/category/gm-world)
* Complete the [Ignite CLI blog tutorial](https://docs.ignite.com/guide/blog)

### Instructions

First, create a Web3.Storage account [here](https://web3.storage/login/) or using the [cli](https://github.com/web3-storage/w3up-cli) (Keep this safe!).

Next, fork [Rollmint](https://github.com/celestiaorg/rollmint). Navigate to ```block/manager.go```. We will need to add [go-w3s-client](https://github.com/web3-storage/go-w3s-client) to the imports and install that package. Another import that needs to be added is ```"bytes"``` We will start writing code at line ~449 (after the block is applied)...

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

Back to the code file at hand...nothing more is changed. After the aforementioned code is run the block is submitted to the DA Layer.

If you've made it this far, kudos! 

In order for the modifications described below to take place, you must publish a release (or prerelease) for the relevant libraries. This can be achieved through Github's UI. First click on the ```Releases``` button

<img width="318" alt="Screen Shot 2022-11-22 at 4 47 11 PM" src="https://user-images.githubusercontent.com/33232379/203427792-78cb1e7d-36dc-4813-ad7e-fe4e721f0b06.png">

You will be asked to choose a "tag", a title, and a target branch. You will also be asked to "describe your release". To acquire your binary, compress the relevant repositories into a zip file and upload on the UI. 

Next we need to fork [cosmos-sdk-rollmint](https://github.com/celestiaorg/cosmos-sdk-rollmint) and modify it [here](https://github.com/celestiaorg/cosmos-sdk-rollmint/blob/release/v0.46.x-rollmint/go.mod#LL12) to depend on our version of rollmint. 

After that, we must alter our ```gm world``` application, which should be completed prior to following these instructions. Modify this application to use YOUR version of cosmos-sdk-rollmint (instead of the version they are telling you to use in the tutorial)



#### Links

* [rollmint](https://github.com/DED-EDU/rollmint)

* [cosmos-sdk-rollmint](https://github.com/DED-EDU/cosmos-sdk-rollmint)

* [gm](https://github.com/DED-EDU/gm)

Note: the repositories above contain distinct differences from celestiaorg repos

##### Future Work

-How to add the filecoin location (CID) to the block header: 

1. Modifiy this [file](https://github.com/celestiaorg/rollmint/blob/7be5aa0e7d795b95edb20da7dd6124d8d95d6275/types/block.go#L58)
2. Modify [this struct](https://github.com/celestiaorg/rollmint/blob/7be5aa0e7d795b95edb20da7dd6124d8d95d6275/types/pb/rollmint/rollmint.pb.go) as well (BUT DON'T MODIFY DIRECTLY!)
3. Modify [this file](https://github.com/celestiaorg/rollmint/blob/7be5aa0e7d795b95edb20da7dd6124d8d95d6275/proto/rollmint/rollmint.proto) and then compile it.

Note: modifying the block header might not be necessary / desirable

* Refactor [w3up-cli](https://github.com/web3-storage/w3up-cli) to Golang, so that users can easily create API tokens programmtically.
