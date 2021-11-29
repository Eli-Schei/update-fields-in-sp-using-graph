<template>
  <div class="container demo-wrapper">
    <h1>{{ msg }}</h1>
    <form id="uploadDocForm" class="container">
      <div class="row">
        <label class="col-3 col-md-3 col-sm-12">Date </label>
        <div class="col-3 col-md-3 col-sm-12">
          <input required id="customDate" type="date" />
        </div>
      </div>
      <div class="row">
        <label class="col-3 col-md-3 col-sm-12">Type:</label>
        <div class="col-3 col-md-3 col-sm-12">
          <select required id="customType" name="customType">
            <option value="" selected disabled hidden>Select a type</option>
            <option
              v-for="(customType, i) in typeValues"
              v-bind:key="'thistype' + i"
              v-bind:value="customType"
            >
              {{ customType }}
            </option>
          </select>
        </div>
      </div>
      <div class="row">
        <label class="col-3 col-md-3 col-sm-12">
            Another value:
        </label>
        <div class="col-3 col-md-3 col-sm-12">
          <input id="anotherValue" name="anotherValue">
        </div>       
      </div>
      <div class="row selectFileArea">
        <div class="col-3 col-md-3 col-sm-12">
          <input id="selectedFile" type="file" />
        </div>
      </div>
      <div class="row">
        <div class="col-sm-12">
          <button type="submit" form="uploadDocForm" v-on:click="onSubmit">
            Upload file to SharePoint
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import { signInWithMsal } from "../authenticatingWithMsal";
import { uploadDocument } from "../uploadingDocWithGraph";
import { typeValues } from "../fieldSelectValues";
export default {
  name: "UploadIncidentForm",
  props: ["msg"],
  setup() {
    const onSubmit = async (event) => {
      event.preventDefault();
      await signInWithMsal();
      const filename = document.getElementById("selectedFile").files[0].name;
      const fileToUplode = document.getElementById("selectedFile").files[0];
      const filereader = new FileReader();
      filereader.onload = async (event) => {
        await uploadDocument(filename, event.target.result);
      };
      filereader.readAsArrayBuffer(fileToUplode);
    };
    return {
      onSubmit,
      typeValues,
    };
  },
};
</script>

<style>
.demo-wrapper {
  text-align: left;
  border: 1px solid grey;
  border-radius: 10px;
  padding: 20px;
}

.demo-wrapper h1 {
  margin-bottom: 30px;
}

.demo-wrapper .row {
  margin-bottom: 15px;
  text-align: left;
}

.demo-wrapper button{
  padding: 10px;
  border-radius: 10px;
  background: #aed1ca;
  color: black;
  border: none;
}
.selectFileArea{
  margin-top: 20px;
  padding-top: 20px;
  border-top: 1px solid #aed1ca;
}
.demo-wrapper button:hover{
  background: #333333;
  color: white;
}
</style>
