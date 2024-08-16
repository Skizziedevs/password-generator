<script setup>
import { ref } from "vue";

const plclicked = ref(false);
const passwordLength = ref(12);
const includeUppercase = ref(false);
const includeLowercase = ref(false);
const includeNumbers = ref(false);
const includeSymbols = ref(false);
const generatedPassword = ref("Select Options");
const passwordStrength = ref("");
const passwordStrengthColor = ref("");

const evaluatePasswordStrength = (password) => {
  const lengthStrength = password.length >= 8 ? "Strong" : "Weak";
  const uppercaseStrength =
    includeUppercase.value && /[A-Z]/.test(password) ? "Strong" : "Weak";
     const lowercaseStrength =
    includeLowercase.value && /[a-z]/.test(password) ? "Strong" : "Weak";
  const numberStrength =
    includeNumbers.value && /\d/.test(password) ? "Strong" : "Weak";

  const overallStrength = [
    lengthStrength,
    uppercaseStrength,
    lowercaseStrength,
    numberStrength,
  ].every((strength) => strength === "Strong")
    ? "Strong"
    : "Weak";

  passwordStrengthColor.value =
    overallStrength === "Strong" ? "#63ff63" : "red";

  return overallStrength;
};

const generatePassword = () => {


   if (!includeUppercase.value && !includeLowercase.value && !includeNumbers.value && !includeSymbols.value) {
    generatedPassword.value = "Select Options";
    return;
  }
  const uppercaseChars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  const lowercaseChars = "abcdefghijklmnopqrstuvwxyz";
  const numberChars = "0123456789";
  const symbolChars = "#$%!&*@";

  let chars = "";

   if (includeLowercase.value) {
    chars += lowercaseChars;
  }

  if (includeUppercase.value) {
    chars += uppercaseChars;
  }

  if (includeNumbers.value) {
    chars += numberChars;
  }
  if (includeSymbols.value) {
    chars += symbolChars;
  }

  let password = "";
  for (let i = 0; i < passwordLength.value; i++) {
    const randomIndex = Math.floor(Math.random() * chars.length);
    password += chars.charAt(randomIndex);
  }

  generatedPassword.value = password;
  console.log(password);

  // Evaluate password strength
  passwordStrength.value = evaluatePasswordStrength(password);
};

const copyToClipboard = () => {
  const passwordInput = document.getElementById("generatedPassword");

  if (!passwordInput) {
    console.error("Password input element not found");
    return;
  }

  passwordInput.select();
  document.execCommand("copy");

  console.log("Password copied to clipboard");
};

const plToggle = () => {
  plclicked.value = !plclicked.value;
  console.log(plclicked.value);
};
</script>

<template>
  <body>
    <div class="main">
      <h3 class="title">Password Generator</h3>
      <div class="first-block">
        <input
          class="pw"
          type="text"
          v-model="generatedPassword"
          readonly
          id="generatedPassword"
        />
        <div @click="copyToClipboard">
          <font-awesome-icon class="copy" icon="fa-regular fa-copy" />
        </div>
      </div>
      <div class="second-block">
        <span class="length">
          <h3 style="font-size: 15px">Character Length</h3>
          <div>
            <input
              v-show="(plclicked = true)"
              @keyup.enter="plclicked = true"
              type="number"
              id="length"
              v-model="passwordLength"
              min="1"
              max="50"
            />
          </div>
        </span>

        <form style="padding-left: 19px; padding-top: 15px">
          <input
            class="checkbox"
            type="checkbox"
            id="includeUppercase"
            v-model="includeUppercase"
            value="uppercase"
          />
          <label for="choice1"> Include Uppercase Letters</label><br />
          <input
            class="checkbox"
            type="checkbox"
            id="includeLowercase"
            value="lowercase"
            v-model="includeLowercase"
          />
          <label for="includeLowerCase"> Include Lowercase Letters</label><br />
          <input
            class="checkbox"
            type="checkbox"
            id="includeNumbers"
            v-model="includeNumbers"
            value="number"
          />
          <label for="choice3"> Include Numbers</label> <br />
          <input
            class="checkbox"
            type="checkbox"
            id="includeSymbols"
            v-model="includeSymbols"
            value="symbol"
          />
          <label for="choice4"> Include Symbols</label>
        </form>
        <div class="strength">
          <h2 style="padding-left: 22px; padding-top: 9px">Strength :</h2>
          <h3
            style="padding-right: 22px; font-size: 16px; padding-top: 13px"
            :style="{ color: passwordStrengthColor }"
          >
            {{ passwordStrength }}
          </h3>
        </div>
        <button @click="generatePassword">Generate</button>
      </div>
    </div>
  </body>
</template>

<style src="./style.css"></style>

<style scoped>
#length {
  background-color: #505050;
  border: none;
  color: #63ff63;
  font-size: 21px;
  font: bold;
  text-decoration: none;
  margin-left: 20px;
  width: 47px;
}
.strength {
  background-color: #3b3939;
  text-align: center;
  height: 45px;
  width: 314px;
  margin-left: 18px;
  position: relative;
  top: 15px;
  display: flex;
  justify-content: space-between;
}

.strength h2 {
  font-size: 20px;
  align-items: center;
}
.pw {
  background-color: #505050;
  border: none;
  padding-top: 4px;
  color: #8a8989;
  font-size: 20px;
  font: bold;
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
}
button {
  width: 314px;
  height: 47px;
  background-color: rgb(99, 255, 99);
  margin-left: 18px;
  border: none;
  position: relative;
  top: 29px;
  cursor: pointer;
}
.copy {
  color: rgb(99, 255, 99);
  font-size: 26px;
  padding-top: 3px;
  cursor: pointer;
}
.hidden {
  display: none;
}
.number {
  color: #63ff63;
}
body {
  height: 100vh;
  width: 100vw;
  background: #000;
}
.checkbox {
  margin-top: 8px;
  margin-bottom: 8px;
}
h1,
h2,
h3,
label {
  color: white;
}

.second-block {
  background-color: #505050;
  height: 330px;
}
.length {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding-left: 18px;
  padding-right: 18px;
  padding-top: 23px;
}
.first-block {
  display: flex;
  flex-direction: row;
  background-color: #505050;
  justify-content: space-between;
  padding-top: 10px;
  padding-bottom: 10px;
  padding-left: 18px;
  padding-right: 18px;
  margin-top: 13px;
  margin-bottom: 13px;
}
.main {
  width: 340px;
  height: 450px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: black;
}
.title {
  text-align: center;
  color: #ccc;
  padding-top: 10px;
  padding-bottom: 10px;
}
.center-box {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff; /* Set your desired background color */
  padding: 20px; /* Set your desired padding */
  border: 1px solid #ccc; /* Set your desired border styles */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Set your desired box shadow */
}
* {
  margin: 0;
  padding: 0;
}
</style>
