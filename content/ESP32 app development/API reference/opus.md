---
title: "opus"
nodateline: true
weight: 9999
---

# Encoding data

To encode data, you have to know the sampling rate and number of channels and create an `Encoder`:

```
import opus
sampling_rate = 8000
stereo = False
encoder = opus.Encoder(sampling_rate, stereo)
```

Then you can use the encoder to encode audio frames. Those may have lengths of 2.5, 5, 10, 20, 40, or 60 milliseconds. Input data should be of type `bytes` or `bytearray` and contain 16-bit signed integers:

```
# One frame of data containing 480 null samples
input = bytearray(960)
# Encode the data
output = encoder.encode(input)
```

Each encoded frame has some metadata at the beginning containing the channel,
frequency, and the encoded size of the frame. This allows combining frames
into one packet.

# Decoding data

Decoders are created just like encoders:

```
import opus
sampling_rate = 8000
stereo = False
decoder = opus.Decoder(sampling_rate, stereo)
```

The created decoder can handle any data created by `opus.Encoder`, even if the
number of channels or the sampling rate differs - it will get reinitialized to
match the new settings.

```
encoder = opus.Encoder(8000, 0)
decoder = opus.Encoder(8000, 0)

input = bytearray(960)
encoded = encoder.encode(input)
decoded = decoder.decode(encoded)
```
