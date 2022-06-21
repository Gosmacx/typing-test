# 🔥 typing-test ⌨️ 

Web application written in vue where you can test your typing speed.

**Live Website:** https://typing-test-gosmac.netlify.app

## How it Works
```js

let currentWord = "Hello" // Word to write.
let currentType = "Hel" // User typed word.

if (currentWord.text.includes(currentType.value.trimEnd())) {
  // The spelled word is neither true nor false.
  // ⚪ Set the background of the word box to white.
} else {
  // Spelled word is wrong.
  // 🔴 Set the background of the word box to red.
}

if (currentType.value.trimEnd() == currentWord.text) {
  // Spelled word is true.
  // 🟢 Set the background of the word box to green.
}


```


## ⚡ Screenshots ⚡

![ss](https://raw.githubusercontent.com/Gosmacx/typing-test/master/screenshots/ss1.png)
![ss](https://raw.githubusercontent.com/Gosmacx/typing-test/master/screenshots/ss2.png)


