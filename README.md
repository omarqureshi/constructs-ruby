# constructs-ruby

Ruby packaging for [`constructs`](https://github.com/aws/constructs), built
with [jsii-target-ruby](https://github.com/omarqureshi/jsii-target-ruby).

`constructs` is the shared base library beneath both `aws-cdk-lib` and
`cdk8s`. It has no jsii dependencies of its own — twelve types, no submodules —
so it generates standalone, which is what makes this repository small.

## Why it is separate

Because it is shared, and shared artifacts need one owner.

Generated packages pin their dependencies exactly
(`JSII_RUBY_PIN_DEPENDENCIES=exact`), so `aws-cdk-lib 2.263.0` requires
`constructs = 10.8.1` and a cdk8s build requires whatever its own closure
resolved. Every one of those versions has to exist as a gem. If each
distribution repository published its own, they would collide on shared
versions — and `gem build` is not byte-reproducible, so the second upload would
change the checksum the feed advertises for a version already in someone's
lockfile.

## Publishing a version

```
Actions → publish → version: 10.5.1, publish: true
```

Any version on npm can be built, which is the point: supporting a library means
supporting the `constructs` version its closure pins, including older ones.
Already-published versions are left alone rather than overwritten.
