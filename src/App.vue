<script setup>
import { onMounted, ref } from "vue";
import Spinner from "./components/Spinner.vue";
import CorrectIcon from "./components/CorrectIcon.vue";

const nameValue = ref("");
const surNameValue = ref("");
const lastNameValue = ref("");
const sendMessage = ref(false);
const citizenship = ref("");
const gender = ref("");

const pending = ref(false);
const posted = ref(false);

const handleSubmit = async () => {
  pending.value = true;
  posted.value = false;

  await fetch("http://localhost:3000/customers", {
    method: "POST",
    headers: {
      "Content-Type": "application/json;charset=utf-8",
    },
    body: JSON.stringify({
      name: nameValue.value,
      surname: surNameValue.value,
      lastname: lastNameValue.value,
      sendMessages: sendMessage.value,
      citizenship: citizenship.value,
      gender: gender.value,
    }),
  });

  nameValue.value = "";
  surNameValue.value = "";
  lastNameValue.value = "";
  sendMessage.value = false;
  citizenship.value = [];
  gender.value = "";
  pending.value = false;
  posted.value = true;

  setTimeout(() => {
    posted.value = false;
  }, 2000);
};

const getData = async () => {
  const res = await fetch("http://localhost:3000/customers");
  const data = await res.json();

  console.log(data);
};

onMounted(() => {
  getData();
});
</script>

<template>
  <div>
    <form @submit.prevent="handleSubmit">
      <template v-if="!pending && !posted">
        <div>
          <label class="label" for="surname">Surname*:</label>
          <input
            required
            type="text"
            id="surname"
            v-model.trim="surNameValue"
          />

          <label class="label" for="name">Name*:</label>
          <input required type="text" id="name" v-model.trim="nameValue" />

          <label class="label" for="lastname">Last Name:</label>
          <input type="text" id="lastname" v-model.trim="lastNameValue" />
        </div>

        <label for="sendMessage">
          Send News:
          <input type="checkbox" id="sendMessage" v-model="sendMessage" />
        </label>

        <div>
          <label class="label" for="citizenship"> Citizenship: </label>
          <select
            multiple
            class="multiselect"
            id="citizenship"
            v-model="citizenship"
          >
            <option selected disabled>Choose citizenship</option>
            <option value="eu">Europe</option>
            <option value="usa">United States</option>
            <option value="cis">CIS</option>
          </select>
        </div>

        <label for="male">
          Male:
          <input type="radio" id="male" value="male" v-model="gender" />
        </label>
        <label for="female">
          Female:
          <input type="radio" id="female" value="female" v-model="gender" />
        </label>
        <button>Submit Data</button>
      </template>
      <template v-else>
        <CorrectIcon v-if="posted"></CorrectIcon>
        <Spinner v-if="pending"></Spinner>
      </template>
    </form>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

form {
  max-width: 480px;
  margin: 0 auto;
  text-align: left;
}
input[type="text"] {
  display: block;
  margin: 10px 0;
  width: 100%;
  box-sizing: border-box;
  padding: 10px;
  border: 1px solid #ccc;

  &.is-invalid {
    border: 1px solid red;
  }
}
input[type="checkbox"] {
  display: inline;
}
textarea {
  height: 160px;
}
.label {
  display: inline-block;
  margin-top: 30px;
  position: relative;
  font-size: 20px;
  color: #fff;
  margin-bottom: 10px;
  padding: 2px 0;
}

.label::before {
  content: "";
  display: block;
  width: 100%;
  height: 100%;
  background: #ff8800;
  position: absolute;
  top: 0;
  z-index: -1;
  padding-right: 40px;
  left: -30px;
  transform: rotateZ(-1deg);
}
button {
  display: block;
  margin-top: 30px;
  background: #ff8800;
  border: none;
  color: #fff;
  padding: 8px 16px;
  font-size: 18px;
  cursor: pointer;
}
button:disabled {
  opacity: 0.3;
  cursor: wait;
}
.pill {
  display: inline-block;
  margin: 10px 10px 0 0;
  color: #444;
  background: #ddd;
  padding: 8px;
  border-radius: 20px;
  cursor: pointer;
  font-size: 14px;
}
.select {
  display: block;
  width: 100%;
  padding: 0.375rem 2.25rem 0.375rem 0.75rem;
  font-size: 1rem;
  font-weight: 400;
  line-height: 1.5;
  color: #212529;
  background-color: #fff;
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16'%3e%3cpath fill='none' stroke='%23343a40' stroke-linecap='round' stroke-linejoin='round' stroke-width='2' d='M2 5l6 6 6-6'/%3e%3c/svg%3e");
  background-repeat: no-repeat;
  background-position: right 0.75rem center;
  background-size: 16px 12px;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
  appearance: none;

  &:focus {
    outline: 0;
  }
}
.multiselect {
  display: block;
  width: 100%;
  padding: 0.375rem 2.25rem 0.375rem 0.75rem;
  -moz-padding-start: calc(0.75rem - 3px);
  font-size: 1rem;
  font-weight: 400;
  line-height: 1.5;
  color: #212529;
  background-color: #fff;
  background-repeat: no-repeat;
  background-position: right 0.75rem center;
  background-size: 16px 12px;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;

  &:focus {
    outline: 0;
  }
}
.warning {
  color: red;
}
select {
  option {
    font-weight: normal;
    display: block;
    min-height: 1.2em;
    padding: 0px 2px 1px;
    white-space: nowrap;
  }
}

.modal {
  width: 500px;
  height: 500px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;

  p {
    font-size: 30px;
  }
}
</style>
