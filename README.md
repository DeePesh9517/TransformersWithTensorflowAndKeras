# Understanding Transformers with Tensorflow and Keras
* Most complex systems, ideas and papers can be traced down to:</br>
<b>"Data, matrix multiplications, repeated and scaled with non-linear switches"</b>
* We started NLP with <b>tokens</b> and then build <b>representations</b> of these tokens.
* Next we use these representations to find similiraties between tokens and we embed them in a hihg-dimensional space.
* After that these embeddings are passed to a model that can process sequential data.
* These models are used to build context and, through an ingenious way, attend to parts of the input sentence that are useful to output sentence in <b>translation</b></br>
## Here we will understand the evolution of attention mechanism that led to the seminal architecture of Transformers.
* We will focus on
    - Encoder
    - Decoder

# The Transformer Architecture
* Let's understand it from top-down to build Transformer Architecture.
* The transformer consits of two individual models.
    - Decoder
    - Encoder
<img src="images/transformer_arch_2.png" width="350" title="transformer Architecture">

## Encoder
* The encoder is a stack of _N_ identical layers.
* Each layer is composed of two sub-layers.
* The first is a **multi-head self-attention mechanism**, and the second is a simple, **postition-wise, fully connected feed-forward network**.
* We can also apply residual connections and a normalization operation around the two-sub layers.
* The source token are first embedded into a high-dimensional space.
* The input embeddings are added with **positional encoding**. The summed embeddings are then fed into encoder.

## Decoder
* The decoder is a stack of _N_ identical layers.
* Each layer is composed of three sub-layers.
* adding to the two sub-layers in encoder, the decoder consists a third sub-layer, which performs multi-head attention over the output of the encoder stack.
* The decoder also contains residual connections and normalization operation around the three sub0layers.
* The first sublayer of decoder is a **masked** multi-headed attention layer instead of a multi-head attention layer.
* The target tokens are offset by one.
* Similar to encoder, the token are embedded into a high-dimensional space.
* Post which the embeddings are then added with positional encodings.
* These summed encodings are then fed into the decoder.
> This __masking__, combined with the fact that the target token are offset by one position, ensures that the prediction _i_ can depend ony on the known outputs at  positions less than _i_
## Evolution of Attention
* The encoder and decoder have been built around a central piece called **Multi-Head Attention** module.
* This piece of the architecture is a the formula X that has placed Transformers at the top of the Deep Learning food Chain.
* But Multi-Head Attention (MHA) did not always exist in its present form.
* However, the journey from the early attention to the one that is actually used in the Transformers architecture is long and full of **monstrous** notations.
### Version 0
* To understand the intution of attentio, we start with an **input** and a **query**.
* Then, we attend to parts of the input based on the query.
* So if you have an image of a landscape and someone asks you to decipher the weather there, you would attend to the sky first.
* The image is the input, while the query is "how is the weather there?"
* In terms of computation, attention is given to parts of the **input matrix** which is similar to the **query vector**.
* We comput the similarity between the input matrix and the query vector.
* After we obtain the **similarity score**, we transform the input matrix into an output vector.
* The output vector is the **weighted summation (or average)** of the input matrix.
* Intutively the weighted summation should be richer in representation thatn the original input matrix.
* It includes the "where and what to attend to".
* The diagram of the baseline version (version 0) is as below.
##### Inputs:
<table>
    <tr>
        <th></th>
        <th>Notation</th>
        <th>Shape</th>
    </tr>
    <tr>
        <td>Query Vector</td>
        <td>q</td>
        <td>(Dq)</td>
    </tr>
    <tr>
        <td>Input Matrix</td>
        <td>X</td>
        <td>(Nx, Dx)</td>
    </tr>
</table>

* **Similarity function**: *f*att, which is a **Feed-forward network**.
* The feed-forward network takes the query and input, and projects both of them to dimension De.

##### Outputs:
<table>
    <tr>
        <th></th>
        <th>Notation</th>
        <th>Equation</th>
        <th>shape</th>
    </tr>
    <tr>
        <td>Similarity Score</td>
        <td>e</td>
        <td>_e_i = _f_att(Xi, q)</td>
        <td>(Nx, De)</td>
    </tr>
    <tr>
        <td>Attention Weights</td>
        <td>a</td>
        <td>a=softmax(e)</td>
        <td>(Nx)</td>
    </tr>
    <tr>
        <td>Output Vector</td>
        <td>y</td>
        <td>y = summation(ai.Xi)</td>
        <td>(Dx)</td>
    </tr>
</table>