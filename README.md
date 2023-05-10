# Arithmatic-Encoder-and-Decoder

## Overview 

Arithmetic encoding is a lossless data compression technique that encodes a message as a single floating-point number between 0 and 1. The encoding process works by dividing the number line into intervals corresponding to the probabilities of each symbol in the message. The interval for each symbol is then divided into sub-intervals based on the probabilities of the symbols that follow it. The resulting sub-intervals are recursively divided until each symbol has a unique sub-interval. The encoded message is the number that falls within the sub-interval for the entire message.

The decoding process involves reversing this process by dividing the interval for the entire message into sub-intervals and identifying the symbol associated with each sub-interval until the entire message is reconstructed. Decoding the encoded message involves computing the decimal fraction (tag) corresponding to the given binary code, constructing an array that contains the cumulative sums of the probabilities starting from 0, and then using these probabilities to identify the symbol associated with each sub-interval in the code.

### Encoder:
  - Start by finding L and H using Find_Range function.
  - Using [L , H] as an input for the Find_Code function to generate a specific code for the given word.
  - In Find_Code function: this function depends on splitting the given range into two parts; lower and upper blocks and according to the given L and H, it tries to find the start and end points of a block that would completely between the values of the L and H. It starts a loop that checks the three cases that can occur while dividing the given range according to the relative positions of the value of the L and H to the start_point, end_point, and half_point of that range:
    1. if the value of H is less than or equal to the half_point of the current range, this means that we can use the lower block to be divided further to find a block that would eventually fit inside L and H. if that block is chosen, ‘0’ is added to the codeword.
    2. if the value of L is greater than or equal to the value of the half_point, following the same logic as above, we can choose the upper block to be further divided. This s done by adding ‘1’ to the codeword.
    3. if L is less than the half_point and H is greater than the half_point, then this means that they got stuck between both lower and upper halves and we would at least require two further divisions of the range to get a suitable block. To decide which of the two blocks: lower or upper would be chosen, we check the difference between H and the half_pont and L and the half_point: chose the greater difference because this means that there is a larger area that can be used to find a block that would fit inside L and H using a smaller number of divisions compared to the other option. Again if the upper block is chosen, add’1’ to the codeword, otherwise add ‘0’ to the codeword.
  - The loop ends when the start_point is greater than or equal to L and the end_point is less than or equal to H.
  - We added an if-condition after the loop, which would further divide the block and choose the lower portion if the end_pont of the current block equals H just to satisfy the condition that the block should be totally included in [L,H).

### Decoder:
- The decoder function takes binary code, symbols, props, and the length of the
sequence as an input, then produces the decode word as an output.
- Start by computing the decimal fraction (tag) corresponding to the given binary
code.
- Based on the given probabilities, we construct an array that contains the
cumulative sums of the probabilities starting from 0 which is called ‘line’.
- Loop on the length of the sequence to generate the desired word. In each
iteration, there is another loop that goes over all the points in the current range of
probabilities and checks which portion of that line includes the given tag, then
adds the corresponding symbol to the decoded word. After that it updates the
‘line1’ array to contain the new range of probabilities depending on the chosen
symbol and the previous range of probabilities.
- After the outer loop, it returns the decoded word for the given code
