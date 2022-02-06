<template>
  <div class="home">
    <div class="container">
      <h1 class="mb-5">Welcome Password Generator</h1>
      <div class="row justify-content-center">
        <div class="col-lg-8">
          <div class="row justify-content-center">
            <div class="col-lg-auto text-start">
              <input
                type="number"
                class="form-control mb-4"
                placeholder="password Length"
                v-model="passLength"
              />
              <label class="switch mb-4">
                <span>Include Symbols ( @#$% etc. )</span>
                <input type="checkbox" v-model="symbols" />
                <span class="slider"></span>
              </label>
              <label class="switch mb-4">
                <span>Include Numbers ( 123456 etc. )</span>
                <input type="checkbox" v-model="numbers" />
                <span class="slider"></span>
              </label>
              <label class="switch mb-4">
                <span>Include Lowercase Characters ( abcdefgh etc. )</span>
                <input type="checkbox" v-model="lowchars" />
                <span class="slider"></span>
              </label>
              <label class="switch mb-4">
                <span>Include Uppercase Characters ( ABCDEFGH etc. )</span>
                <input type="checkbox" v-model="upchars" />
                <span class="slider"></span>
              </label>
              <div class="input-group mb-4">
                <input
                  type="text"
                  v-bind:value="password"
                  readonly
                  v-on:focus="$event.target.select()"
                  ref="myinput"
                  class="form-control form-control-lg"
                  placeholder="Generated password"
                />
                <button
                  class="btn btn-outline-secondary"
                  type="button"
                  @click="copyURL()"
                >
                  Copy
                </button>
              </div>
              <div class="text-center">
                <button
                  type="button"
                  @click="generatePassword()"
                  class="btn btn-primary"
                >
                  Generate Password
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-lg-8 mx-auto mt-5 mb-5">
        <table class="table table-bordered">
          <thead>
            <tr>
              <th>#index</th>
              <th>#password</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="o in oldPasswords" :key="o">
              <td>
                {{ o.id }}
              </td>
              <td>
                {{ o.pass }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component({
  components: {},
})
export default class Home extends Vue {
  private password = "";
  private symbols = true;
  private passLength = 15;
  private numbers = true;
  private lowchars = false;
  private upchars = false;
  private oldPasswords = [] as any;
  private counter = 0;

  private generatePassword() {
    const symbolData = "()_-{}',.~=|+:[]\"<>\\/&@#^;%$*?!";
    const numberData = "0123456789";
    const upperCharsData = "ABCDEFGHIJKLMNOPQRSTUVWXTZ";
    const lowerCharsData = "abcdefghiklmnopqrstuvwxyz";
    let generatePassword = "";
    if (this.symbols) {
      generatePassword += symbolData;
    }
    if (this.numbers) {
      generatePassword += numberData;
    }
    if (this.lowchars) {
      generatePassword += lowerCharsData;
    }
    if (this.upchars) {
      generatePassword += upperCharsData;
    }
    var a = generatePassword.split("").sort(() => Math.random() - 0.5);
    for (var i = 0; i <= this.passLength; i++) {
      var k = Math.floor(Math.random() * (i + 1));
      var temp = a[i];
      a[i] = a[k];
      a[k] = temp;
    }
    this.counter++;
    this.password = a.join("").substr(0, this.passLength);
    this.oldPasswords.push({ id: this.counter, pass: this.password });
  }

  private copyURL() {
    this.$refs.myinput.focus();
    document.execCommand("copy");
  }
}
</script>

<style lang="scss">
:root {
  --theme-color: #0a58ca;
}
.table {
  tr td {
    width: 50%;
    &:first-child {
      width: 10%;
    }
  }
}
.switch {
  position: relative;
  display: block;
  padding-left: 50px;
  height: 16px;
  span:not(.slider) {
    position: relative;
    top: -2px;
    cursor: pointer;
  }
  input {
    opacity: 0;
    width: 0;
    height: 0;
    &:checked + .slider {
      background-color: #4caf50;
      &:before {
        transform: translateX(20px);
      }
    }
  }
  .slider {
    position: absolute;
    cursor: pointer;
    width: 42px;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    border-radius: 34px;
    background-color: #f44336;
    transition: 0.4s;
    &:before {
      position: absolute;
      content: "";
      height: 24px;
      width: 24px;
      left: -1px;
      bottom: -4px;
      background-color: #fff;
      transition: 0.4s;
      border-radius: 50%;
      box-shadow: 0 3px 1px -2px rgba(0, 0, 0, 0.2),
        0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 1px 5px 0 rgba(0, 0, 0, 0.12);
    }
  }
}
</style>
