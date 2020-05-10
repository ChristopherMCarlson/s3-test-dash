// src/components/newPost.vue
<template>
  <v-row class='mx-5 my-5'>
    <v-col cols='9'>
      <v-card outlined>
        <v-form @submit="createPost">
          <h2 class="text-center">New Post</h2>
            <v-text-field class='px-12' v-model='form.title' label="Title" />
            <v-textarea class='px-12' v-model='form.body' label="Post Body" />
          </v-form>
        </v-card>
      </v-col>
        <v-col cols='3'>
          <v-card outlined>
            <v-row>
              <v-col cols="12">
                <v-img :src="imageData"></v-img>
                <input @change="previewImage" class='mx-auto' type="file" accept="image/*">Upload Image</input>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12">
                <v-btn v-on:click='createPost' class='mx-auto'>Create Post</v-btn>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
      </v-row>
</template>

<script>
    export default {
      name: 'newPost',
      data() {
        return {
          form: {
            title: '',
            body: '',
            image: ''
          },
          imageToUpload: null,
          imageData: ''
        }
      },
      methods: {
        createPost() {
          console.log(JSON.stringify(this.form)); 
        },
        previewImage(event) {
          // Reference to the DOM input element
          var input = event.target;
          // Ensure that you have a file before attempting to read it
          if (input.files && input.files[0]) {
              // create a new FileReader to read this image and convert to base64 format
              var reader = new FileReader();
              // Define a callback function to run, when FileReader finishes its job
              reader.onload = (e) => {
                  // Note: arrow function used here, so that "this.imageData" refers to the imageData of Vue component
                  // Read image as base64 and set to imageData
                  this.imageData = e.target.result;
                  this.form.image = e.target.result;
              }
              // Start the reader job - read file as a data url (base64 format)
              reader.readAsDataURL(input.files[0]);
          }
      }
      }
    }
</script>