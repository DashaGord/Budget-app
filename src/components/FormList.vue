<template>
  <ElCard class="form-card">
    <ElForm :model="newPost" ref="postForm" :rules="rules" label-position="top">
      <ElFormItem label="Type" prop="type">
        <ElSelect class="type-select" v-model="newPost.type" placeholder="Choose type...">
          <ElOption label="Income" value="INCOME" />
          <ElOption label="Outcome" value="OUTCOME" />
        </ElSelect>
      </ElFormItem>
      <ElFormItem label="Comments" prop="comment">
        <ElInput v-model="newPost.comment" placeholder="Write a comment"/>
      </ElFormItem>
      <ElFormItem label="Value" prop="value">
        <ElInput v-model.number="newPost.value" placeholder="Write a sum"/>
      </ElFormItem>
      <ElButton @click="onSubmit" type="primary">Submit</ElButton>
    </ElForm>
  </ElCard>
</template>

<script>
export default {
  name: "FormList",
  data() {
    return {
      newPost: {
        type: '',
        comment: '',
        value: '',
      },
      rules: {
        type: [
          { required: true, message: "Please select type", trigger: "blur" }
        ],
        comment: [
          { required: true, message: "Please input comment", trigger: "change",
            validator: (rule, value, callback) => {
              if (!this.submitted && !value) {
                callback(new Error('Please input comment'));
              } else {
                callback();
              }
            }
          }
        ],
        value: [
          { required: true, message: "Please input value (not zero)", trigger: "change" ,
            validator: (rule, value, callback) => {
              if (!this.submitted && !value || value === 0) {
                callback(new Error('Please input value (not zero)'));
              } else {
                callback();
              }
            }
          },
          { type: "number", message: "Value must be a number", trigger: "change" }
        ]
      },
      submitted: false
    };
  },
  methods: {
    onSubmit() {
      this.$refs.postForm.validate(valid => {
        if (valid) {
          this.submitForm();
          this.submitted = true;
        } else {
          this.$message.error('Please fill in all fields.');
        }
      });
    },
    submitForm() {
      this.newPost.id = Date.now();
      this.checkTransaction(this.newPost);
      this.$emit('submitForm', this.newPost)
      this.newPost = {
        type: '',
        comment: '',
        value: ''
      }
      this.submitted = false;
    },
    checkTransaction (newPost) {
      if(newPost.type === 'INCOME'){
        newPost.value = Math.abs(newPost.value);
      }
      if(newPost.type === 'OUTCOME'){
        newPost.value = -Math.abs(newPost.value);
      }
    },
  }
};
</script>

<style scoped>
.form-card {
  max-width: 500px;
  margin: auto;
}

.type-select {
  width: 100%;
}
</style>