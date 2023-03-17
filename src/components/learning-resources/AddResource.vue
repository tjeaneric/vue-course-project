<script setup>
import { ref, onMounted, inject } from 'vue'

const AddResource = inject('addResourceData')

const titleInput = ref(null)
const descInput = ref(null)
const linkInput = ref(null)
const inputIsInvalid = ref(false)
const submitData = () => {
  const enteredTitle = titleInput.value.value
  const enteredDesc = descInput.value.value
  const enteredLink = linkInput.value.value

  if (enteredTitle.trim() === '' || enteredDesc.trim() === '' || enteredLink.trim() === '') {
    inputIsInvalid.value = true
    return
  }
  AddResource(enteredTitle, enteredDesc, enteredLink)
}

onMounted(() => {
  titleInput.value.focus()
  descInput.value.focus()
  linkInput.value.focus()
})

const confirmError = () => (inputIsInvalid.value = false)
</script>

<template>
  <base-dialog v-if="inputIsInvalid" title="Invalid input" @close="confirmError">
    <template #default>
      <p>Unfortunately, One input value is invalid.</p>
      <p>Please make sure you enter at least a few characters.</p>
    </template>
    <template #actions>
      <base-button @click="confirmError">Okay</base-button>
    </template>
  </base-dialog>
  <base-card>
    <form @submit.prevent="submitData">
      <div class="form-control">
        <label for="title">Title</label>
        <input type="text" name="title" id="title" ref="titleInput" />
      </div>
      <div class="form-control">
        <label for="description">Descriptiom</label>
        <textarea name="description" id="description" cols="30" rows="3" ref="descInput"></textarea>
      </div>

      <div class="form-control">
        <label for="link">Link</label>
        <input type="url" name="link" id="link" ref="linkInput" />
      </div>
      <div>
        <base-button type="submit">Add Resource</base-button>
      </div>
    </form>
  </base-card>
</template>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>
