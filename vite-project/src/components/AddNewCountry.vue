<template>
  <Button label="Add New Country"  size="large" @click="toggleModal"/>
  <br />
  <!-- <br /> -->
  <div>
    <Dialog
      v-model:visible="visible"
      modal
      header="Add New Country"
      :style="{ width: '35rem' }"
    >
      <br />

      <!-- {{ country }} -->
      <div class="flex flex-wrap justify-content-center">
        <form @submit.prevent="addCountry">
          <div class="card flex col gap-4 justify-center">
            <FloatLabel>
              <InputText id="username" v-model="country.name" class="input" />
              <label for="username">Name</label>
            </FloatLabel>
            <br />
            <br />
            <FloatLabel>
              <InputText
                id="username"
                v-model="country.continent"
                class="input"
              />
              <label for="username">Continent</label>
            </FloatLabel>

            <br />
            <br />
            <FloatLabel>
              <InputNumber id="username" v-model="country.rank" class="input" />
              <label for="username">Rank</label>
            </FloatLabel>
            <br />
            <br />
            <FloatLabel>
              <inputText
                id="username"
                type="file"
                @change="onChange"
                class="input"
              />
              <!-- <FileUpload mode="basic" name="demo[]" url="/api/upload" accept="image/*" :maxFileSize="1000000" @upload="onUpload" /> -->

              <!-- <label for="username">Flag</label> -->
            </FloatLabel>
          </div>
          <br/>
          <div class="justify-content-center">
            <Button label="Add Country" type="submit" />
          </div>
        </form>
      </div>
    </Dialog>
  </div>
</template>

<script setup>
import { ref } from "vue";

import FileUpload from "primevue/fileupload";
import FloatLabel from "primevue/floatlabel";
import InputNumber from "primevue/inputnumber";
import Button from "primevue/button";
import InputText from "primevue/inputtext";
import Dialog from "primevue/dialog";
import axios from "axios";

const visible = ref(false);
const emit = defineEmits(['success'])
const country = ref({
  name: "",
  continent: "",
  rank: 0,
  flag: "",
});

// const visible = ref(false)

const toggleModal = () => {
  visible.value = !visible.value
  console.log('Modal toggled:', visible.value)
}

const onChange = (ev) => {
  country.value.flag = ev.target.files[0];
};
async function addCountry() {
  try {
    const formData = new FormData();

    // Append country data
    Object.keys(country.value).forEach((key) => {
      formData.append(key, country.value[key]);
    });

    const response = await axios.post(
      `http://localhost:8080/country`,
      formData,
      {
        headers: {
          "Content-Type": "multipart/form-data",
        },
      }
    );
      // Countries.value.push(response)
    visible.value = false;
    emit('success')
  } catch (error) {
    console.error("Error adding new country:", error.message);
    throw error;
  }
}
</script>
