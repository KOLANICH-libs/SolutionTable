SolutionTable [![Unlicensed work](https://raw.githubusercontent.com/unlicense/unlicense.org/master/static/favicon.png)](https://unlicense.org/)
=============
~~[wheel (GitLab)](https://gitlab.com/KOLANICH-libs/SolutionTable.py/-/jobs/artifacts/master/raw/dist/SolutionTable-0.CI-py3-none-any.whl?job=build)~~
~~[wheel (GHA via `nightly.link`)](https://nightly.link/KOLANICH-libs/SolutionTable.py/workflows/CI/master/SolutionTable-0.CI-py3-none-any.whl)~~
~~![GitLab Build Status](https://gitlab.com/KOLANICH-libs/SolutionTable.py/badges/master/pipeline.svg)~~
~~![GitLab Coverage](https://gitlab.com/KOLANICH-libs/SolutionTable.py/badges/master/coverage.svg)~~
~~[![GitHub Actions](https://github.com/KOLANICH-libs/SolutionTable.py/workflows/CI/badge.svg)](https://github.com/KOLANICH-libs/SolutionTable.py/actions/)~~
[![Libraries.io Status](https://img.shields.io/librariesio/github/KOLANICH-libs/SolutionTable.py.svg)](https://libraries.io/github/KOLANICH-libs/SolutionTable.py)
[![Code style: antiflash](https://img.shields.io/badge/code%20style-antiflash-FFF.svg)](https://codeberg.org/KOLANICH-tools/antiflash.py)

**We have moved to https://codeberg.org/KOLANICH-libs/SolutionTable.py, grab new versions there.**

Under the disguise of "better security" Micro$oft-owned GitHub has [discriminated users of 1FA passwords](https://github.blog/2023-03-09-raising-the-bar-for-software-security-github-2fa-begins-march-13/) while having commercial interest in success and wide adoption of [FIDO 1FA specifications](https://fidoalliance.org/specifications/download/) and [Windows Hello implementation](https://support.microsoft.com/en-us/windows/passkeys-in-windows-301c8944-5ea2-452b-9886-97e4d2ef4422) which [it promotes as a replacement for passwords](https://github.blog/2023-07-12-introducing-passwordless-authentication-on-github-com/). It will result in dire consequencies and is competely inacceptable, [read why](https://codeberg.org/KOLANICH/Fuck-GuanTEEnomo).

If you don't want to participate in harming yourself, it is recommended to follow the lead and migrate somewhere away of GitHub and Micro$oft. Here is [the list of alternatives and rationales to do it](https://github.com/orgs/community/discussions/49869). If they delete the discussion, there are certain well-known places where you can get a copy of it. [Read why you should also leave GitHub](https://codeberg.org/KOLANICH/Fuck-GuanTEEnomo).

---

This library was initially written in 2013 for analyzing Mafia/Werewolf games but it is suitable to solving problems.
it has 3 main methods:
* `.init([one set],[another set])` - initializes table columns and rows
* `.aIsNotB("element of the first set","element of the second set")` - used when it is known that 2 elements differ
* `.removePairAndResolve(name1, name2)` - used when it is known that 2 elements are equal

Look the examples (the problems were taken from http://mmmf.msu.ru/archive/20052006/z5/11.html) .

Problem 1 - Clowns
------------------
The three clown: `Bim`, `Bom` and `Bam` act in `green`, `red` and `blue` shirts.
Their shoes are of the same colors.
Colors of `Bim`'s shoes and shirt match.
Neither `Bom`'s shirt, nor shoes are `red`.
`Bam` wears `green` shoes and a shirt of a different color.
How are the clowns dressed? (All the clowns wear different shirts and different shoes).

```js
var a = new SolutionTable();
var shoes={}, shirts={};
a.onResolved.push(console.info);
a.onResolved.push(function(v){for(let n in v) shoes[n]=v[n];});

// prbs/clowns_shoes.prb
a.init(["Bim","Bom","Bam"],["r","g","b"]);
a.isNot("Bom","r");  // Neither Bom's shirt, nor **shoes are red**.
a.equal("Bam","g");  // Bam wears green shoes

a.onResolved[a.onResolved.length-1]=(function(v){for(let n in v) shirts[n]=v[n];});
a.init(["Bim", "Bom", "Bam"],["r", "g", "b"]);
a.isNot("Bom", "r");//**Neither Bom's shirt**, nor shoes **are red**.
a.isNot("Bam", "g");//Bam wears green shoes
a.equal("Bim", shoes["Bim"]);//Colors of Bim's shoes and shirt match.
console.log(shoes,shirts);
```

Problem 2 - Plant
-----------------

One plant employs three friends: `turner`, `fitter` and `welder`. Their surnames are `Borisov`, `Ivanov` and `Semenov`.
The `fitter` has neither brothers, nor sisters. He is the youngest of the friends.
`Semenov` is married to `Borisov`'s sister and he is older than the `turner`. Match the surnames to the occupations.

```js
var a = new SolutionTable();
a.onResolved.push(console.info);
// prbs/plant.prb
a.init(["turner", "fitter", "welder"], ["Borisov", "Ivanov", "Semenov"]);
a.isNot("fitter","Borisov"); // fitter has no sisters, Borisov has
a.isNot("turner","Semenov"); // Semenov is older than the turner
a.isNot("fitter", "Semenov"); // fitter is the youngest, Semenov is older than someone
console.log(a.hash1,a.hash2);
```

[Mafia Game](https://en.wikipedia.org/wiki/Mafia_%28party_game%29)
------------------------------------------------------------------
Initially this was developed to reveal roles from mafia game log

```js
var a = new SolutionTable();
a.onResolved.push(console.info);
a.init(["maniac", "maf", "doc", "psy"], ["Paul", "Vlad", "Nick", "Paul"]);
var alone = a.equal("doc", "Paul"); // we know he is doc
var alone = a.equal("maniac", "Paul"); // you need to be careful - no checks are implemented, so this will delete maniac column though Paul cannot be a maniac and this can be seen from the table.
console.log(alone,a.hash1,a.hash2);

// prbs/mafia_2.prb
a.init(["maniac", "maf", "doc", "psy"],["Paul", "Vlad", "Nick", "Paul"]);
var alone = a.isNot("doc", "Paul");
var alone = a.isNot("maniac", "Paul");
var alone = a.isNot("maniac", "Vlad");
var alone = a.isNot("psy", "Vlad");
var alone = a.isNot("psy", "Nick");
var alone = a.isNot("maniac", "Nick");
console.log(alone, a.hash1, a.hash2);
```

Python
------

This repo also contains some python classes. They are not finished yet.

* `SolutionTable.py` contains my try to implement the same as in `SolutionTable.js`, but using Bayessian approach.
* `prbs` contains machine-readable problems in a form of `*.prb` files. You need [`prb.py`](https://codeberg.org/KOLANICH-libs/prb.py) to read them. You can also open them with [Logic Problem Solver](http://delphiforfun.org/Programs/logic_problem_solver.htm) by Gary Darby. The archives with the tool contain quite some pre-formalized examples, that may be used in future to test this library.
