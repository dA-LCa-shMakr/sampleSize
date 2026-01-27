# browser-detect

<p align="center">
  <img src="./sample.png" />
</p>

![tone](https://img.shields.io/badge/tone-minimal-2b2b2b)
![stack](https://img.shields.io/badge/stack-webgl%20runtime-ff6f00)

---

Runs a detection model straight in the tab, no server round trip. The weights are served through the browser runtime with the `webgl` backend, so a laptop GPU does most of the work.

**Getting the code**

```bash
git clone https://github.com/example-org/browser-detect.git
cd browser-detect
yarn install --frozen-lockfile
```

**Commands**

```bash
yarn dev     # hot-reload server on port 3000
yarn bundle  # minified output in dist/
```

## Weights

The shipped checkpoint is a small detector already converted to the browser format.

```
used model : detector-s
ukuran     : 13 Mb
```

**Swapping the checkpoint**

Any checkpoint from the same family works, as long as it is exported to the browser format first.

1. Export your checkpoint. The upstream docs walk through the flags.

   ```python
   from detector import Net

   # Instantiate the network
   net = Net("detector-s.pt")  # load an official model

   # Write out the browser bundle
   net.export(format="tfjs")
   ```

2. Copy the exported `*_web_model` folder to `./public`
3. Point `modelName` in `App.jsx` at the new folder
   ```jsx
   ...
   // model configs
   const modelName = "detector-*"; // points at the exported folder
   ...
   ```
4. That is all.

**Note: your own classes**

Update `src/utils/labels.json` to match the labels you trained on.

## References

- the upstream detector project
- the onnx runtime port of the same app