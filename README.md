# LittleClusters

_Build distributed systems from scratch._

This repository contains the documentation, challenge definitions, and guides that power [littleclusters.com](https://littleclusters.com).

## Structure

```
content/
├── challenges/
│   └── kv-store/          # KV store challenge stages
├── guides/                # Getting started guides
└── ...
```

## Contributing

Found a typo? Want to improve a challenge description?

1. Fork this repository
2. Make your changes
3. Submit a pull request

For major changes or new challenges, please open an issue first to discuss.

## Local Development

This site is built with [Hugo](https://gohugo.io/). To run locally:

```console
$ git clone https://github.com/littleclusters/docs.git
$ cd docs
$ git submodule update --init
$ hugo server -D
```

Visit [http://localhost:1313](http://localhost:1313) to preview.
