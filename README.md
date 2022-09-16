[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/edudip)](https://artifacthub.io/packages/search?repo=edudip)

# The edudip GmbH Helm Chart Repository

Some wholesome applications as [Helm Charts](https://helm.sh/docs/) for your [Kubernetes]() cluster, ready to go.

## Add Repository

```bash
$ helm repo add edudip https://charts.edudip.dev/
$ helm search repo edudip
$ helm install <release> edudip/<chart>
```

## Prerequisites

- Kubernetes 1.21+
- Helm 3.9.4+

## License

MIT License

Copyright (c) 2022 edudip GmbH

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.