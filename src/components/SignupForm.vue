<template>
  <form @submit.prevent="handleSubmit">
    <label>Email:</label>
    <input type="email" required v-model="email" />

    <label>Password:</label>
    <input type="password" required v-model="password" />
    <div v-if="passwordError" class="error">{{ passwordError }}</div>

    <label>Role</label>
    <select v-model="role">
      <option value="developer">Web Developer</option>
      <option value="designer">Web Designer</option>
    </select>

    <label>Skills</label>
    <input type="text" v-model="tempSkill" @keyup.alt="addSkill">
    <div v-for="skill in skills" :key="skill" class="pill">
        <span  @click="deleteSkill(skill)">{{ skill }}</span>
    </div>

    <div class="terms">
      <input type="checkbox" required v-model="terms" />
      <label>Accept terms and condition</label>
    </div>
   
     <div class="submit">
        <button>Create an Account</button>   
    </div>
  </form>
  <p>Email: {{ email }}</p>
  <p>Password: {{ password }}</p>
  <p>Role: {{ role }}</p>
  <p>Terms: {{ terms }}</p>
</template>

<script setup lang="ts">
import type { Ref } from 'vue';
import { ref } from 'vue'

const email: Ref<string> = ref('')
const password: Ref<string> = ref('')
const role: Ref<string> = ref('designer')
const terms: Ref<boolean> = ref(false)
const tempSkill: Ref<string> = ref('')
const skills: Ref<string[]> = ref([])
const passwordError: Ref<string> = ref('')

const addSkill = (e: KeyboardEvent) => {
    if(e.key === ',' && tempSkill){
        if(!skills.value.includes(tempSkill.value)){
            skills.value.push(tempSkill.value)
        }
        tempSkill.value = ''
    }
}

const deleteSkill = (skill:string) => {
    skills.value = skills.value.filter(item => skill !== item)
}

const handleSubmit = () => {
    // validate password
    passwordError.value = password.value.length > 5  ? '' : 'Password must be at least 6 chars long'
    if(!passwordError.value){
        console.log('email ', email.value)
        console.log('password ', password.value)
        console.log('role ', role.value)
        console.log('skills ', skills.value)
        console.log('terms accepted ', terms.value)
    }
}
</script>

<style scoped>
form {
  max-width: 420px;
  margin: 30px auto;
  background: white;
  text-align: left;
  padding: 40px;
  border-radius: 10px;
}
label {
  color: #aaa;
  display: inline-block;
  margin: 25px 0 15px;
  font-size: 0.6em;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: bold;
}
input,
select {
  display: block;
  padding: 10px 6px;
  width: 100%;
  box-sizing: border-box;
  border: none;
  border-bottom: 1px solid #ddd;
  color: #555;
}
input[type='checkbox'] {
  display: inline-block;
  width: 16px;
  margin: 0 10px 0 0;
  position: relative;
}
.pill{
    display: inline-block;
    margin: 20px 10px 0 0;
    padding: 6px 12px;
    background: #eee;
    border-radius: 20px;
    font-size: 12px;
    letter-spacing: 1px;
    font-weight: bold;
    color: #777;
    cursor: pointer;
}
button{
    background: #0b6dff;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    color: white;
    border-radius: 20px;
}
.submit{
    text-align: center;
}
.error{
    color: #ff0062;
    margin-top: 10px;
    font-size: 0.8em;
    font-weight: bold;
}
p {
  text-align: center;
}
</style>
