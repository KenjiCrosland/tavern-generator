<template>
  <div>
    <button @click="updateDescription">Generate New Description</button>
    <p>{{ generatedDescription }}</p>
  </div>
</template>
<script>
import { ref, computed } from 'vue';
import _ from 'lodash-es';
import descriptions from './descriptions.json';

export default {
  setup() {

    const descriptionsRef = ref(descriptions);
    const randomValueFromArray = (template, data) => {
      let output = template;
      for (const key in data) {
        if (key !== 'template') {
          const value = data[key];
          const randomValue = value[Math.floor(Math.random() * value.length)];
          output = output.replace(`{{${key}}}`, randomValue);
        }
      }
      return output;
    };
    const generateDescription = () => {
      const descriptionSet = _.sample(_.keys(descriptionsRef.value));
      const generalDescription = randomValueFromArray(descriptionsRef.value[descriptionSet].generalDescriptions[0].template, descriptionsRef.value[descriptionSet].generalDescriptions[0]);
      const atmosphericDetail = randomValueFromArray(descriptionsRef.value[descriptionSet].atmosphericDetails[0].template, descriptionsRef.value[descriptionSet].atmosphericDetails[0]);
      return `${generalDescription} ${atmosphericDetail}`;
    };
    const generatedDescription = ref(generateDescription());
    const updateDescription = ()=> {
      generatedDescription.value = generateDescription();

    }
    



    return {
      updateDescription,
      generatedDescription,
      generateDescription,
    };
  },
};
</script>