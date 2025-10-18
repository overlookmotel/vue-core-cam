# Oxlint JS plugin benchmarks


## Oxlint 1.23.0

These benchmarks are flawed. There's a bug in Oxlint, causing JS plugins not to be run on many files.
Need to use latest main branch which includes the bug fix (see below).

```sh
hyperfine -i --warmup 3 \
  './node_modules/.bin/oxlint --silent' \
  './node_modules/.bin/oxlint -c .oxlintrc-with-custom-plugin.json --silent' \
  'USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint .' \
  'USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint . --concurrency=auto'
```

### Macbook Air M3, 24 GB RAM

```
Benchmark 1: ./node_modules/.bin/oxlint --silent
  Time (mean ± σ):      52.4 ms ±   1.1 ms    [User: 149.8 ms, System: 24.4 ms]
  Range (min … max):    51.0 ms …  57.7 ms    56 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ./node_modules/.bin/oxlint -c .oxlintrc-with-custom-plugin.json --silent
  Time (mean ± σ):      64.0 ms ±   2.2 ms    [User: 172.7 ms, System: 27.6 ms]
  Range (min … max):    59.3 ms …  70.4 ms    46 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint .
  Time (mean ± σ):      4.151 s ±  0.088 s    [User: 8.170 s, System: 0.735 s]
  Range (min … max):    4.037 s …  4.294 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 4: USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint . --concurrency=auto
  Time (mean ± σ):      3.452 s ±  0.088 s    [User: 18.440 s, System: 1.757 s]
  Range (min … max):    3.335 s …  3.617 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ./node_modules/.bin/oxlint --silent ran
    1.22 ± 0.05 times faster than ./node_modules/.bin/oxlint -c .oxlintrc-with-custom-plugin.json --silent
   65.92 ± 2.15 times faster than USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint . --concurrency=auto
   79.26 ± 2.33 times faster than USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint .
```

### Mac Mini M4 Pro, 48 GB RAM

```
Benchmark 1: ./node_modules/.bin/oxlint --silent
  Time (mean ± σ):      56.1 ms ±   0.7 ms    [User: 131.0 ms, System: 37.5 ms]
  Range (min … max):    54.6 ms …  57.8 ms    50 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ./node_modules/.bin/oxlint -c .oxlintrc-with-custom-plugin.json --silent
  Time (mean ± σ):      68.9 ms ±   1.5 ms    [User: 150.7 ms, System: 43.4 ms]
  Range (min … max):    66.9 ms …  74.2 ms    43 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint .
  Time (mean ± σ):      3.879 s ±  0.028 s    [User: 6.813 s, System: 1.101 s]
  Range (min … max):    3.842 s …  3.930 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 4: USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint . --concurrency=auto
  Time (mean ± σ):      2.590 s ±  0.092 s    [User: 16.597 s, System: 3.750 s]
  Range (min … max):    2.437 s …  2.700 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ./node_modules/.bin/oxlint --silent ran
    1.23 ± 0.03 times faster than ./node_modules/.bin/oxlint -c .oxlintrc-with-custom-plugin.json --silent
   46.15 ± 1.76 times faster than USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint . --concurrency=auto
   69.12 ± 1.05 times faster than USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint .
```


## Oxlint main @ cd266b4c101c35c33e122457cdd0b514b44597a9 (between 1.23.0 and 1.24.0)

Same as above, but using latest main oxlint.
Latest main has bug fixes which ensure JS plugins run on every file when config contains `overrides`.

```sh
hyperfine -i --warmup 3 \
  'node ../../crates/oxc/apps/oxlint/dist/cli.js --silent' \
  'node ../../crates/oxc/apps/oxlint/dist/cli.js -c .oxlintrc-with-custom-plugin.json --silent' \
  'USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint .' \
  'USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint . --concurrency=auto'
```

### Macbook Air M3, 24 GB RAM

```
Benchmark 1: node ../../crates/oxc/apps/oxlint/dist/cli.js --silent
  Time (mean ± σ):      48.2 ms ±   1.1 ms    [User: 141.9 ms, System: 23.7 ms]
  Range (min … max):    46.3 ms …  53.2 ms    62 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: node ../../crates/oxc/apps/oxlint/dist/cli.js -c .oxlintrc-with-custom-plugin.json --silent
  Time (mean ± σ):     236.3 ms ±   9.6 ms    [User: 661.8 ms, System: 44.8 ms]
  Range (min … max):   223.6 ms … 260.4 ms    11 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint .
  Time (mean ± σ):      4.116 s ±  0.073 s    [User: 8.086 s, System: 0.719 s]
  Range (min … max):    4.045 s …  4.300 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 4: USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint . --concurrency=auto
  Time (mean ± σ):      3.710 s ±  0.147 s    [User: 19.363 s, System: 1.919 s]
  Range (min … max):    3.540 s …  4.038 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  node ../../crates/oxc/apps/oxlint/dist/cli.js --silent ran
    4.90 ± 0.23 times faster than node ../../crates/oxc/apps/oxlint/dist/cli.js -c .oxlintrc-with-custom-plugin.json --silent
   77.01 ± 3.54 times faster than USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint . --concurrency=auto
   85.43 ± 2.49 times faster than USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint .
```

### Mac Mini M4 Pro, 48 GB RAM

```
Benchmark 1: node ../../crates/oxc/apps/oxlint/dist/cli.js --silent
  Time (mean ± σ):      47.6 ms ±   1.2 ms    [User: 125.1 ms, System: 35.4 ms]
  Range (min … max):    45.2 ms …  50.6 ms    59 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: node ../../crates/oxc/apps/oxlint/dist/cli.js -c .oxlintrc-with-custom-plugin.json --silent
  Time (mean ± σ):     221.2 ms ±   6.0 ms    [User: 570.5 ms, System: 53.7 ms]
  Range (min … max):   210.1 ms … 229.1 ms    13 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint .
  Time (mean ± σ):      3.867 s ±  0.029 s    [User: 6.783 s, System: 1.087 s]
  Range (min … max):    3.826 s …  3.907 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 4: USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint . --concurrency=auto
  Time (mean ± σ):      2.569 s ±  0.102 s    [User: 16.327 s, System: 3.760 s]
  Range (min … max):    2.444 s …  2.730 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  node ../../crates/oxc/apps/oxlint/dist/cli.js --silent ran
    4.65 ± 0.17 times faster than node ../../crates/oxc/apps/oxlint/dist/cli.js -c .oxlintrc-with-custom-plugin.json --silent
   54.01 ± 2.51 times faster than USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint . --concurrency=auto
   81.28 ± 2.06 times faster than USE_CUSTOM_PLUGIN=true ./node_modules/.bin/eslint .
```
