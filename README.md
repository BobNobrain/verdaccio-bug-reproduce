# verdaccio 404 bug reproducement

To reproduce, do:

```bash
docker-compose up -d
cd pkg1
npm login --registry=http://localhost:4873
npm publish --registry=http://localhost:4873
```

Registry credentials are `lupa:pupa`.

## Expected

The package should be published to registry as a brand new package named `pkg1-by-lupa`.

## Actual

### verdaccio log from docker-compose:

```
verdaccio    |  http --> 404, req: 'GET https://registry.npmjs.org/pkg1-by-lupa' (streaming)
verdaccio    |  http --> 404, req: 'GET https://registry.npmjs.org/pkg1-by-lupa', bytes: 0/21
verdaccio    |  http <-- 404, user: lupa(172.20.0.1), req: 'PUT /pkg1-by-lupa', error: no such package available
verdaccio    |  http <-- 404, user: lupa(172.20.0.1), req: 'PUT /pkg1-by-lupa', error: no such package available
```

### npm log:

```
0 info it worked if it ends with ok
1 verbose cli [ '/home/bob/bin/node',
1 verbose cli   '/home/bob/bin/npm',
1 verbose cli   'publish',
1 verbose cli   '--registry=http://localhost:4873' ]
2 info using npm@6.4.1
3 info using node@v8.11.1
4 verbose npm-session a718d4c2bb799f75
5 verbose publish [ '.' ]
6 info lifecycle pkg1-by-lupa@1.0.0~prepublish: pkg1-by-lupa@1.0.0
7 info lifecycle pkg1-by-lupa@1.0.0~prepare: pkg1-by-lupa@1.0.0
8 info lifecycle pkg1-by-lupa@1.0.0~prepublishOnly: pkg1-by-lupa@1.0.0
9 info lifecycle pkg1-by-lupa@1.0.0~prepack: pkg1-by-lupa@1.0.0
10 info lifecycle pkg1-by-lupa@1.0.0~postpack: pkg1-by-lupa@1.0.0
11 notice
12 notice 📦  pkg1-by-lupa@1.0.0
13 notice === Tarball Contents ===
14 notice 230B package.json
15 notice === Tarball Details ===
16 notice name:          pkg1-by-lupa
16 notice version:       1.0.0
16 notice package size:  259 B
16 notice unpacked size: 230 B
16 notice shasum:        1764461f58bdb5a6f202561cdb717be245e8d08a
16 notice integrity:     sha512-/qVagYywuOsGE[...]/E1Kz5f/GNNXA==
16 notice total files:   1
17 notice
18 verbose getPublishConfig undefined
19 silly mapToRegistry name pkg1-by-lupa
20 silly mapToRegistry using default registry
21 silly mapToRegistry registry http://localhost:4873/
22 silly mapToRegistry data { type: 'tag',
22 silly mapToRegistry   registry: true,
22 silly mapToRegistry   where: undefined,
22 silly mapToRegistry   raw: 'pkg1-by-lupa',
22 silly mapToRegistry   name: 'pkg1-by-lupa',
22 silly mapToRegistry   escapedName: 'pkg1-by-lupa',
22 silly mapToRegistry   scope: undefined,
22 silly mapToRegistry   rawSpec: '',
22 silly mapToRegistry   saveSpec: null,
22 silly mapToRegistry   fetchSpec: 'latest',
22 silly mapToRegistry   gitRange: undefined,
22 silly mapToRegistry   gitCommittish: undefined,
22 silly mapToRegistry   hosted: undefined }
23 silly mapToRegistry uri http://localhost:4873/pkg1-by-lupa
24 verbose publish registryBase http://localhost:4873/
25 silly publish uploading /tmp/npm-29620-f6224841/tmp/fromDir-1e0ac9b0/package.tgz
26 verbose request uri http://localhost:4873/pkg1-by-lupa
27 verbose request sending authorization for write operation
28 info attempt registry request try #1 at 11:05:01
29 verbose request using bearer token for auth
30 verbose request id 90c215c5493f0515
31 http request PUT http://localhost:4873/pkg1-by-lupa
32 http 404 http://localhost:4873/pkg1-by-lupa
33 verbose headers { 'x-powered-by': 'verdaccio/3.8.5',
33 verbose headers   'access-control-allow-origin': '*',
33 verbose headers   'content-type': 'application/json; charset=utf-8',
33 verbose headers   'content-length': '43',
33 verbose headers   etag: 'W/"2b-qLjXXPpa5cmKw8bM3gatrZVKHm8"',
33 verbose headers   vary: 'Accept-Encoding',
33 verbose headers   'x-status-cat': 'http://flic.kr/p/aV6juR',
33 verbose headers   date: 'Tue, 23 Oct 2018 08:05:03 GMT',
33 verbose headers   connection: 'keep-alive' }
34 error publish Failed PUT 404
35 verbose stack Error: no such package available : pkg1-by-lupa
35 verbose stack     at makeError (/home/bob/lib/node_modules/npm/node_modules/npm-registry-client/lib/request.js:329:12)
35 verbose stack     at RegClient.<anonymous> (/home/bob/lib/node_modules/npm/node_modules/npm-registry-client/lib/request.js:317:14)
35 verbose stack     at Request._callback (/home/bob/lib/node_modules/npm/node_modules/npm-registry-client/lib/request.js:216:14)
35 verbose stack     at Request.self.callback (/home/bob/lib/node_modules/npm/node_modules/request/request.js:185:22)
35 verbose stack     at emitTwo (events.js:126:13)
35 verbose stack     at Request.emit (events.js:214:7)
35 verbose stack     at Request.<anonymous> (/home/bob/lib/node_modules/npm/node_modules/request/request.js:1161:10)
35 verbose stack     at emitOne (events.js:116:13)
35 verbose stack     at Request.emit (events.js:211:7)
35 verbose stack     at IncomingMessage.<anonymous> (/home/bob/lib/node_modules/npm/node_modules/request/request.js:1083:12)
35 verbose stack     at Object.onceWrapper (events.js:313:30)
35 verbose stack     at emitNone (events.js:111:20)
35 verbose stack     at IncomingMessage.emit (events.js:208:7)
35 verbose stack     at endReadableNT (_stream_readable.js:1064:12)
35 verbose stack     at _combinedTickCallback (internal/process/next_tick.js:138:11)
35 verbose stack     at process._tickCallback (internal/process/next_tick.js:180:9)
36 verbose statusCode 404
37 verbose pkgid pkg1-by-lupa
38 verbose cwd /home/bob/git/work/kiann/verdaccio-test/pkg1
39 verbose Linux 4.15.0-38-generic
40 verbose argv "/home/bob/bin/node" "/home/bob/bin/npm" "publish" "--registry=http://localhost:4873"
41 verbose node v8.11.1
42 verbose npm  v6.4.1
43 error code E404
44 error 404 no such package available : pkg1-by-lupa
45 error 404
46 error 404 'pkg1-by-lupa' is not in the npm registry.
47 error 404 You should bug the author to publish it (or use the name yourself!)
48 error 404 Note that you can also install from a
49 error 404 tarball, folder, http url, or git url.
50 verbose exit [ 1, true ]
```
