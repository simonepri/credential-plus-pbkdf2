<h1 align="center">
  <b>phc-pbkdf2</b>
</h1>
<p align="center">
  <!-- CI - TravisCI -->
  <a href="https://travis-ci.org/simonepri/phc-pbkdf2">
    <img src="https://img.shields.io/travis/simonepri/phc-pbkdf2/master.svg?label=MacOS%20%26%20Linux" alt="Mac/Linux Build Status" />
  </a>
  <!-- CI - AppVeyor -->
  <a href="https://ci.appveyor.com/project/simonepri/phc-pbkdf2">
    <img src="https://img.shields.io/appveyor/ci/simonepri/phc-pbkdf2/master.svg?label=Windows" alt="Windows Build status" />
  </a>
  <!-- Coverage - Codecov -->
  <a href="https://codecov.io/gh/simonepri/phc-pbkdf2">
    <img src="https://img.shields.io/codecov/c/github/simonepri/phc-pbkdf2/master.svg" alt="Codecov Coverage report" />
  </a>
  <!-- DM - Snyk -->
  <a href="https://snyk.io/test/github/simonepri/phc-pbkdf2?targetFile=package.json">
    <img src="https://snyk.io/test/github/simonepri/phc-pbkdf2/badge.svg?targetFile=package.json" alt="Known Vulnerabilities" />
  </a>
  <!-- DM - David -->
  <a href="https://david-dm.org/simonepri/phc-pbkdf2">
    <img src="https://david-dm.org/simonepri/phc-pbkdf2/status.svg" alt="Dependency Status" />
  </a>

  <br/>

  <!-- Code Style - XO-Prettier -->
  <a href="https://github.com/xojs/xo">
    <img src="https://img.shields.io/badge/code_style-XO+Prettier-5ed9c7.svg" alt="XO Code Style used" />
  </a>
  <!-- Test Runner - AVA -->
  <a href="https://github.com/avajs/ava">
    <img src="https://img.shields.io/badge/test_runner-AVA-fb3170.svg" alt="AVA Test Runner used" />
  </a>
  <!-- Test Coverage - Istanbul -->
  <a href="https://github.com/istanbuljs/nyc">
    <img src="https://img.shields.io/badge/test_coverage-NYC-fec606.svg" alt="Istanbul Test Coverage used" />
  </a>
  <!-- Init - ni -->
  <a href="https://github.com/simonepri/ni">
    <img src="https://img.shields.io/badge/initialized_with-ni-e74c3c.svg" alt="NI Scaffolding System used" />
  </a>
  <!-- Release - np -->
  <a href="https://github.com/sindresorhus/np">
    <img src="https://img.shields.io/badge/released_with-np-6c8784.svg" alt="NP Release System used" />
  </a>

  <br/>

  <!-- Version - npm -->
  <a href="https://www.npmjs.com/package/@phc/pbkdf2">
    <img src="https://img.shields.io/npm/v/@phc/pbkdf2.svg" alt="Latest version on npm" />
  </a>
  <!-- License - MIT -->
  <a href="https://github.com/simonepri/phc-pbkdf2/tree/master/license">
    <img src="https://img.shields.io/github/license/simonepri/phc-pbkdf2.svg" alt="Project license" />
  </a>
</p>
<p align="center">
  🔒 Node.JS PBKDF2 password hashing algorithm following the PHC string format.
  <br/>

  <sub>
    Coded with ❤️ by <a href="#authors">Simone Primarosa</a>.
  </sub>
</p>

## Synopsis

Protects against brute force, rainbow tables, and timing attacks.

Employs cryptographically secure, per password salts to prevent rainbow table
attacks.  
Key stretching is used to make brute force attacks impractical.  
A constant time verification check prevents variable response time attacks.

## PHC String Format

The [PHC String Format][specs:phc] is an attempt to specify a common hash string format that’s a restricted & well defined subset of the Modular Crypt Format. New hashes are strongly encouraged to adhere to the PHC specification, rather than the much looser [Modular Crypt Format][specs:mcf].

The hash strings generated by this package are in the following format:

```c
$pbkdf2-<digest>$i=<iterations>$<salt>$<hash>
```

Where:

| Field | Type | Description
| --- | --- | --- |
| `<digest>` | <code>string</code> | The [HMAC][specs:HMAC] digest algorithm applied to derive a key of the input password. |
| `<iterations>` | <code>number</code> | The number of iterations desired. The higher the number of iterations, the more secure the derived key will be, but will take a longer amount of time to complete. |
| `<salt>` | <code>string</code> | A sequence of bits, known as a [cryptographic salt][specs:salt] encoded in [B64][specs:B64]. |
| `<hash>` | <code>string</code> | The computed derived key by the [pbkdf2][specs:PBKDF2] algorithm encoded in [B64][specs:B64]. |

## Install

```bash
npm install --save @phc/pbkdf2
```

## Usage

```js
const pbkdf2 = require('@phc/pbkdf2');

// Hash and verify with pbkdf2 and default configs
const hash = await pbkdf2.hash('password');
// => $pbkdf2-sha512$i=10000$O484sW7giRw+nt5WVnp15w$jEUMVZ9adB+63ko/8Dr9oB1jWdndpVVQ65xRlT+tA1GTKcJ7BWlTjdaiILzZAhIPEtgTImKvbgnu8TS/ZrjKgA

const match = await pbkdf2.verify(hash, 'password');
// => true

const match = await pbkdf2.verify(hash, 'wrong');
// => false

const ids = pbkdf2.identifiers();
// => ['pbkdf2-sha1', 'pbkdf2-sha256', 'pbkdf2-sha512']
```

## Benchmarks

Below you can find usage statistics of this hashing algorithm with different
options.  
This should help you understand how the different options affects the running
time and memory usage of the algorithm.

Usage reports are generated thanks to [sympact][gh:sympact].

#### System Report

```
Distro    Release  Platform  Arch
--------  -------  --------  ----
Mac OS X  10.12.6  darwin    x64

CPU     Brand           Clock     Cores
------  --------------  --------  -----
Intel®  Core™ i5-6360U  2.00 GHz  4    

Memory                  Type    Size         Clock   
----------------------  ------  -----------  --------
Micron Technology Inc.  LPDDR3  4294.967 MB  1867 MHz
Micron Technology Inc.  LPDDR3  4294.967 MB  1867 MHz
```

#### Custom iterations

<details>
<summary><strong>1˙000 iterations ↴</strong></summary>

```
CPU Usage (avarage ± σ)  CPU Usage Range (min … max)
-----------------------  ---------------------------
1.70 % ± 1.00 %          0.70 % … 2.70 %            

RAM Usage (avarage ± σ)  RAM Usage Range (min … max)
-----------------------  ---------------------------
23.601 MB ± 0.561 MB     23.040 MB … 24.162 MB      

Execution time  Sampling time  Samples  
--------------  -------------  ---------
0.010 s         0.06 s         2 samples

Instant  CPU Usage  RAM Usage  PIDS
-------  ---------  ---------  -----
0.028 s  0.70 %     23.040 MB  96698
0.060 s  2.70 %     24.162 MB  96698
```

</details>

<details>
<summary><strong>10˙000 iterations ↴</strong></summary>

```
  CPU Usage (avarage ± σ)  CPU Usage Range (min … max)
  -----------------------  ---------------------------
  0.50 % ± 0.00 %          0.50 % … 0.50 %            

  RAM Usage (avarage ± σ)  RAM Usage Range (min … max)
  -----------------------  ---------------------------
  23.562 MB ± 0.543 MB     23.020 MB … 24.105 MB      

  Execution time  Sampling time  Samples  
  --------------  -------------  ---------
  0.021 s         0.069 s        2 samples

  Instant  CPU Usage  RAM Usage  PIDS
  -------  ---------  ---------  -----
  0.027 s  0.50 %     23.020 MB  96709
  0.069 s  0.50 %     24.105 MB  96709
```

</details>

<details>
<summary><strong>25˙000 iterations ↴</strong></summary>

```
CPU Usage (avarage ± σ)  CPU Usage Range (min … max)
-----------------------  ---------------------------
0.90 % ± 0.00 %          0.90 % … 0.90 %            

RAM Usage (avarage ± σ)  RAM Usage Range (min … max)
-----------------------  ---------------------------
23.966 MB ± 0.516 MB     23.237 MB … 24.330 MB      

Execution time  Sampling time  Samples  
--------------  -------------  ---------
0.043 s         0.093 s        3 samples

Instant  CPU Usage  RAM Usage  PIDS
-------  ---------  ---------  -----
0.027 s  0.90 %     23.237 MB  96720
0.078 s  0.90 %     24.330 MB  96720
0.093 s  0.90 %     24.330 MB  96720
```

</details>

<details>
<summary><strong>50˙000 iterations ↴</strong></summary>

```
CPU Usage (avarage ± σ)  CPU Usage Range (min … max)
-----------------------  ---------------------------
0.90 % ± 0.00 %          0.90 % … 0.90 %            

RAM Usage (avarage ± σ)  RAM Usage Range (min … max)
-----------------------  ---------------------------
24.047 MB ± 0.451 MB     23.265 MB … 24.314 MB      

Execution time  Sampling time  Samples  
--------------  -------------  ---------
0.072 s         0.126 s        4 samples

Instant  CPU Usage  RAM Usage  PIDS
-------  ---------  ---------  -----
0.027 s  0.90 %     23.265 MB  96733
0.075 s  0.90 %     24.293 MB  96733
0.108 s  0.90 %     24.314 MB  96733
0.126 s  0.90 %     24.314 MB  96733
```

</details>

<details>
<summary><strong>100˙000 iterations ↴</strong></summary>

```
CPU Usage (avarage ± σ)  CPU Usage Range (min … max)
-----------------------  ---------------------------
15.65 % ± 17.27 %        0.70 % … 40.00 %           

RAM Usage (avarage ± σ)  RAM Usage Range (min … max)
-----------------------  ---------------------------
24.246 MB ± 0.389 MB     23.376 MB … 24.437 MB      

Execution time  Sampling time  Samples  
--------------  -------------  ---------
0.142 s         0.192 s        6 samples

Instant  CPU Usage  RAM Usage  PIDS
-------  ---------  ---------  -----
0.028 s  0.70 %     23.376 MB  96748
0.079 s  4.40 %     24.416 MB  96748
0.111 s  4.40 %     24.416 MB  96748
0.142 s  4.40 %     24.416 MB  96748
0.168 s  40.00 %    24.416 MB  96748
0.192 s  40.00 %    24.437 MB  96748
```

</details>

<details>
<summary><strong>250˙000 iterations ↴</strong></summary>

```
CPU Usage (avarage ± σ)  CPU Usage Range (min … max)
-----------------------  ---------------------------
38.83 % ± 23.16 %        0.60 % … 68.10 %           

RAM Usage (avarage ± σ)  RAM Usage Range (min … max)
-----------------------  ---------------------------
24.286 MB ± 0.304 MB     23.192 MB … 24.388 MB      

Execution time  Sampling time  Samples   
--------------  -------------  ----------
0.368 s         0.42 s         14 samples

Instant  CPU Usage  RAM Usage  PIDS
-------  ---------  ---------  -----
0.028 s  0.60 %     23.192 MB  96767
0.075 s  0.60 %     24.367 MB  96767
0.105 s  20.90 %    24.367 MB  96767
0.136 s  20.90 %    24.367 MB  96767
0.166 s  20.90 %    24.367 MB  96767
0.197 s  20.90 %    24.367 MB  96767
0.229 s  50.90 %    24.367 MB  96767
0.262 s  50.90 %    24.367 MB  96767
0.289 s  50.90 %    24.367 MB  96767
0.319 s  50.90 %    24.367 MB  96767
0.346 s  50.90 %    24.367 MB  96767
0.378 s  68.10 %    24.367 MB  96767
0.404 s  68.10 %    24.388 MB  96767
0.420 s  68.10 %    24.388 MB  96767
```

</details>

<details>
<summary><strong>500˙000 iterations ↴</strong></summary>

```
CPU Usage (avarage ± σ)  CPU Usage Range (min … max)
-----------------------  ---------------------------
61.37 % ± 28.77 %        0.70 % … 91.30 %           

RAM Usage (avarage ± σ)  RAM Usage Range (min … max)
-----------------------  ---------------------------
24.189 MB ± 0.225 MB     23.044 MB … 24.252 MB      

Execution time  Sampling time  Samples   
--------------  -------------  ----------
0.748 s         0.798 s        27 samples

Instant  CPU Usage  RAM Usage  PIDS
-------  ---------  ---------  -----
0.027 s  0.70 %     23.044 MB  96802
0.077 s  13.60 %    24.232 MB  96802
0.107 s  13.60 %    24.232 MB  96802
0.139 s  13.60 %    24.232 MB  96802
0.169 s  13.60 %    24.232 MB  96802
0.198 s  45.10 %    24.232 MB  96802
0.229 s  45.10 %    24.232 MB  96802
0.262 s  45.10 %    24.232 MB  96802
0.289 s  45.10 %    24.232 MB  96802
0.313 s  45.10 %    24.232 MB  96802
0.343 s  65.20 %    24.232 MB  96802
0.373 s  65.20 %    24.232 MB  96802
0.404 s  65.20 %    24.232 MB  96802
0.431 s  65.20 %    24.232 MB  96802
0.462 s  78.20 %    24.232 MB  96802
0.491 s  78.20 %    24.232 MB  96802
0.518 s  78.20 %    24.232 MB  96802
0.547 s  78.20 %    24.232 MB  96802
0.578 s  86.60 %    24.232 MB  96802
0.609 s  86.60 %    24.232 MB  96802
0.639 s  86.60 %    24.232 MB  96802
0.668 s  86.60 %    24.232 MB  96802
0.701 s  91.30 %    24.232 MB  96802
0.727 s  91.30 %    24.232 MB  96802
0.756 s  91.30 %    24.232 MB  96802
0.787 s  91.30 %    24.252 MB  96802
0.798 s  91.30 %    24.252 MB  96802
```

</details>

## API

#### TOC

<dl>
<dt><a href="#hash">hash(password, [options])</a> ⇒ <code>Promise.&lt;string&gt;</code></dt>
<dd><p>Computes the hash string of the given password in the PHC format using Node&#39;s
built-in crypto.randomBytes() and crypto.pbkdf2().</p>
</dd>
<dt><a href="#verify">verify(password, phcstr)</a> ⇒ <code>Promise.&lt;boolean&gt;</code></dt>
<dd><p>Determines whether or not the hash stored inside the PHC formatted string
matches the hash generated for the password provided.</p>
</dd>
<dt><a href="#identifiers">identifiers()</a> ⇒ <code>Array.&lt;string&gt;</code></dt>
<dd><p>Gets the list of all identifiers supported by this hashing function.</p>
</dd>
</dl>

<a name="hash"></a>

### hash(password, [options]) ⇒ <code>Promise.&lt;string&gt;</code>
Computes the hash string of the given password in the PHC format using Node's
built-in crypto.randomBytes() and crypto.pbkdf2().

**Kind**: global function  
**Returns**: <code>Promise.&lt;string&gt;</code> - The generated secure hash string in the PHC
format.  
**Access**: public  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| password | <code>string</code> |  | The password to hash. |
| [options] | <code>Object</code> |  | Optional configurations related to the hashing function. |
| [options.iterations] | <code>number</code> | <code>25000</code> | Optional number of iterations to use. Must be an integer within the range (`1` <= `iterations` <= `2^32-1`). |
| [options.saltSize] | <code>number</code> | <code>16</code> | Optional number of bytes to use when autogenerating new salts. Must be an integer within the range (`1` <= `saltSize` <= `2^10-1`). |
| [options.digest] | <code>string</code> | <code>&quot;sha512&quot;</code> | Optinal name of digest to use when applying the key derivation function. Can be one of [`'sha1'`, `'sha256'`, `'sha512'`]. |

<a name="verify"></a>

### verify(password, phcstr) ⇒ <code>Promise.&lt;boolean&gt;</code>
Determines whether or not the hash stored inside the PHC formatted string
matches the hash generated for the password provided.

**Kind**: global function  
**Returns**: <code>Promise.&lt;boolean&gt;</code> - A boolean that is true if the hash computed
for the password matches.  
**Access**: public  

| Param | Type | Description |
| --- | --- | --- |
| password | <code>string</code> | User's password input. |
| phcstr | <code>string</code> | Secure hash string generated from this package. |

<a name="identifiers"></a>

### identifiers() ⇒ <code>Array.&lt;string&gt;</code>
Gets the list of all identifiers supported by this hashing function.

**Kind**: global function  
**Returns**: <code>Array.&lt;string&gt;</code> - A list of identifiers supported by this
hashing function.  
**Access**: public

## Related
- [@phc/argon2][argon2] -
🔒 Node.JS Argon2 password hashing algorithm following the PHC string format.
- [@phc/scrypt][scrypt] -
🔒 Node.JS scrypt password hashing algorithm following the PHC string format.
- [@phc/bcrypt][bcrypt] -
🔒 Node.JS bcrypt password hashing algorithm following the PHC string format.

## Contributing

Contributions are REALLY welcome and if you find a security flaw in this code, PLEASE [report it][new issue].  
Please check the [contributing guidelines][contributing] for more details. Thanks!

## Authors

- **Simone Primarosa** - *Github* ([@simonepri][github:simonepri]) • *Twitter* ([@simoneprimarosa][twitter:simoneprimarosa])

See also the list of [contributors][contributors] who participated in this project.

## License

This project is licensed under the MIT License - see the [license][license] file for details.

<!-- Links -->
[start]: https://github.com/simonepri/phc-pbkdf2#start-of-content
[new issue]: https://github.com/simonepri/phc-pbkdf2/issues/new
[contributors]: https://github.com/simonepri/phc-pbkdf2/contributors

[license]: https://github.com/simonepri/phc-pbkdf2/tree/master/license
[contributing]: https://github.com/simonepri/phc-pbkdf2/tree/master/.github/contributing.md

[argon2]: https://github.com/simonepri/phc-argon2
[scrypt]: https://github.com/simonepri/phc-scrypt
[bcrypt]: https://github.com/simonepri/phc-bcrypt

[github:simonepri]: https://github.com/simonepri
[twitter:simoneprimarosa]: http://twitter.com/intent/user?screen_name=simoneprimarosa

[gh:sympact]: https://github.com/simonepri/sympact

[specs:mcf]: https://github.com/ademarre/binary-mcf
[specs:phc]: https://github.com/P-H-C/phc-string-format/blob/master/phc-sf-spec.md
[specs:B64]: https://github.com/P-H-C/phc-string-format/blob/master/phc-sf-spec.md#b64
[specs:salt]: https://en.wikipedia.org/wiki/Salt_(cryptography)
[specs:HMAC]: https://en.wikipedia.org/wiki/HMAC
[specs:PBKDF2]: https://en.wikipedia.org/wiki/PBKDF2
