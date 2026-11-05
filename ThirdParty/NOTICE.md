# Bundled local punctuation components

The application performs punctuation inference locally. These files do not add
a network service, require credentials, or upload text/audio. Only English uses
the added model; other source languages retain the existing local rules.

- sherpa-onnx v2.0.1 C API, Apache-2.0: https://github.com/k2-fsa/sherpa-onnx/tree/v2.0.1
- ONNX Runtime v1.28.2, MIT: https://github.com/microsoft/onnxruntime/tree/v1.28.2
  (version reported by the bundled sherpa-onnx runtime). Its bundled third-party
  notices are retained in `ONNX-Runtime-ThirdPartyNotices.txt`.
- Edge-Punct-Casing model, Apache-2.0 project: https://github.com/frankyoujian/Edge-Punct-Casing
- Converted English CNN-BiLSTM model and vocabulary: https://github.com/k2-fsa/sherpa-onnx/releases/tag/punctuation-models
- Model documentation: https://k2-fsa.github.io/sherpa/onnx/punctuation/pretrained_models.html
- Original model card (Apache-2.0): https://huggingface.co/frankyoujian/Edge-Punct-Casing

Runtime packaging modification: the sherpa-onnx library's ONNX Runtime load
path is changed from `@rpath/libonnxruntime.dylib` to
`@loader_path/libonnxruntime.dylib`; both libraries are locally ad-hoc signed.
Model weights and vocabulary are copied unchanged from the pinned archive.

The runtime archive and model archive are pinned by SHA-256 in
`scripts/prepare-punctuation.py`. The app bundles the int8 model and vocabulary;
there is no automatic download during normal use. Local source modifications
are the optional C bridge, punctuation validation, and streaming coordinator.

Punctuation prediction is not a guarantee of complete meaning. The application
validates original-word preservation, waits for following context and stable
predictions, and rejects some visibly incomplete constructions.
