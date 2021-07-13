# [proposal-string-trim-characters](https://github.com/Kingwl/proposal-string-trim-characters)

Proposal to add argument for `.trim()`, `.trimStart()` and `.trimEnd()` to allow strip the specified characters from strings.

## Status

This proposal is a [stage-0 proposal](https://github.com/tc39/proposals/blob/master/stage-0-proposals.md) and waiting for feedback.

## Motivation

We often remove some leading/trailing whitespaces from the beginning or end (or both) of a string by `trim`, `trimStart`, and `trimEnd`.

We can only remove the [WhiteSpace](https://262.ecma-international.org/11.0/#prod-WhiteSpace) and [LineTerminator](https://262.ecma-international.org/11.0/#prod-LineTerminator). If you want to remove some other strings who not defined by whitespace. There's no semantical and convenience way to do that.

### Semantical & Convenience
The major point of the proposal is **semantical** and **convenience**.

For now. How could we remove some specific leading/trailing characters?

- regex

```ts
const str = "-_-abc-_-";
const characters = "-_";

const regex = new RegExp(`(^[${characters}]*)|([${characters}]*$)`, 'g')

console.log(str.replaceAll(regex, '')) // abc
```

- manually

```ts
const str = "-_-abc-_-";
const characters = "-_";

let start = 0;
while (characters.indexOf(str[start]) >= 0) {
    start += 1;
}
let end = str.length - 1;
while (characters.indexOf(str[end]) >= 0) {
    end -= 1;
}

console.log(str.substr(start, end - start + 1)) // abc
```

As you can see. For these both solutions, there's no idea about what does it doing when you see it, You have to pay attention on it to understand it. 

### Performance

And for the regex version, there's might also performance issue as [TypeScript's implementations](https://github.com/microsoft/TypeScript/blob/main/src/compiler/core.ts#L2330-L2344): [jsbench](https://jsbench.me/gjkoxld4au/1).

### Consolation

That's why we need this proposal. It's will add some **semantic** and **convenience** way to `clearly representing the operation i want`. And as a possible bonus, it also reduces the amount of very poorly performing code we write.

## Core API

Add an optional argument `characters` into `String.prototype.trim` ,  `String.prototype.trimStart`and   `String.prototype.trimEnd`. 

This argument will allow us which characters will be remove from the start or end (or both) from the string.

The definition of API will looks like:

```ts
interface String {
    trim(characters?: string): string;
    trimStart(characters?: string): string;
    trimEnd(characters?: string): string;
}

```

With this proposal, we could use as:

```typescript
const str = "-_-abc-_-";
const characters = "-_";

console.log(str.trim(characters)) // abc
console.log(str.trimStart(characters)) // abc-_-
console.log(str.trimEnd(characters)) // -_-abc
```

## Prior art

- Lodash - [lodash.trim](https://lodash.com/docs/4.17.15#trim), [lodash.trimStart](https://lodash.com/docs/4.17.15#trimStart), [lodash.trimEnd](https://lodash.com/docs/4.17.15#trimEnd)
- PHP - [function.trim](https://www.php.net/manual/en/function.trim.php), [function.ltrim](https://www.php.net/manual/en/function.ltrim.php), [function.rtrim](https://www.php.net/manual/en/function.rtrim.php)
- Python - [str.strip](https://docs.python.org/3/library/stdtypes.html#str.strip), [str.lstrip](https://docs.python.org/3/library/stdtypes.html#str.lstrip), [str.rstrip](https://docs.python.org/3/library/stdtypes.html#str.rstrip)
- C# - [String.Trim](https://docs.microsoft.com/en-us/dotnet/api/system.string.trim?view=net-5.0), [String.TrimStart](https://docs.microsoft.com/en-us/dotnet/api/system.string.trimstart?view=net-5.0), [String.TrimEnd](https://docs.microsoft.com/en-us/dotnet/api/system.string.trimend?view=net-5.0)
- Go - [Trim](https://golang.org/pkg/strings/#Trim), [TrimLeft](https://golang.org/pkg/strings/#TrimLeft), [TrimRight](https://golang.org/pkg/strings/#TrimRight), [TrimPrefix](https://golang.org/pkg/strings/#TrimPrefix), [TrimSuffix](https://golang.org/pkg/strings/#TrimSuffix)

## Previous discuss
- https://github.com/tc39/proposal-string-left-right-trim/issues/22 
- https://esdiscuss.org/topic/string-trim-chars 
- https://esdiscuss.org/topic/string-prototype-trimstart-string-prototype-trimend-with-a-given-string

## Proposer

Champions:

- @Kingwl (Wenlu Wang, KWL)

