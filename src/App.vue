<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const isLoading = ref(false)
const isUpdated = ref(false)
const isValid = ref(true)

const firstname = ref('')
const lastname = ref('')
const email = ref('')
const errors = ref({})


const fullname = computed(() => {
  return `${firstname.value} ${lastname.value}`
})

const validateName = (name) => {
  const re = /\d/
  return !re.test(name)
}

const validateEmail = (email) => {
  return email.includes('@')
}

watch([firstname, lastname, email], () => {
  isValid.value = true
  isLoading.value = false
  errors.value = {}


  if (!validateName(firstname.value)) {
    isValid.value = false
    errors.value.firstname = 'ชื่อจริงไม่ถูกต้อง'
  }

  if (!validateName(lastname.value)) {
    isValid.value = false
    errors.value.lastname = 'นามสกุลไม่ถูกต้อง'
  }

  if (!validateEmail(email.value)) {
    isValid.value = false
    errors.value.email = 'อีเมลไม่ถูกต้อง'
  }
})


onMounted(() => {
  firstname.value = 'name'
  lastname.value = 'lastname'
  email.value = 'TEST@gmail.com'
})

const updateProfile = async () => {
  isLoading.value = true
  await (new Promise(resolve => setTimeout(resolve, 2000)))
  isLoading.value = false
  isUpdated.value = true
  await (new Promise(resolve => setTimeout(resolve, 2000)))
  isUpdated.value = false

}

</script>

<template>
  <div class="container">
    <div>
      <div>Fullname : {{ fullname }}</div>
      <div>Email : {{ email }}</div>
    </div>
    <div>
      <div>Firstname</div>
      <input type="text" v-model="firstname">
      <div class="error"> {{ errors.firstname }}</div>

    </div>
    <div>
      <div>Lastname</div>
      <input type="text" v-model="lastname">
      <div class="error"> {{ errors.lastname }}</div>
    </div>
    <div>
      <div>Email</div>
      <input type="text" v-model="email">
      <div class="error"> {{ errors.email }}</div>
    </div>
    <div v-if="isLoading" class="loading">
      Loading.....
    </div>
    <button :disabled="!isValid" class="button" @click="updateProfile()">Update Profile</button>
    <div v-if="isUpdated">
      Profile is updated.
    </div>
  </div>
</template>

<style>
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  max-width: 320px;
  margin: 0 auto;
}

.container>div {
  width: 100%;
}

input {
  width: 100%;
}

.loading {
  background-color: aliceblue;
  width: 100%;
  padding: 20px;
  box-sizing: border-box;
  margin: 10px 0;
}

.button {
  width: 100%;
  height: 24px;
  margin: 15px 0;
}

.error {
color: red;
}

</style>