<template>
  <q-page class="tw-max-w-screen-xl tw-mx-auto tw-px-4 tw-flex tw-flex-col">
    <div class="tw-flex tw-gap-4 tw-py-4">
      <q-input class="tw-grow" label="Prefix" outlined type="text" v-model="prefix" @update:model-value="applyPrefix"></q-input>
      <q-input class="tw-grow" outlined type="text" v-model="prefix"></q-input>

    </div>
    <div class="tw-flex items-stretch tw-gap-4 tw-pb-4 tw-grow ">
      <q-input class="tw-grow" placeholder="Paste your code here . . ." @update:model-value="applyPrefix" outlined type="textarea" v-model="html"></q-input>
      <q-input class="tw-grow" placeholder="Output will be here . . ." outlined type="textarea" readonly v-model="prefixed"></q-input>
    </div>
  </q-page>
</template>

<script>
import { defineComponent,ref } from 'vue'
import classes from "/public/classes.json"
export default defineComponent({
  name: 'Tailwind Prefixer',
  setup(){
    const all_classes = ref(classes.classes)
    return {
      all_classes,
      prefix:ref('tw-'),
      html:ref(''),
      prefixed:ref(''),
    }
  },
  methods:{
    applyPrefix() {
      let self = this
      const escapeRegExp = (s) => s.replace(/[-/\\^$*+?.()|[\]{}]/g, '\\$&');
      let code = self.html;
      self.all_classes.forEach((cls) => {
        code = code.replace(
          new RegExp(
            `(["':\\s])(?!${self.prefix})(-?${escapeRegExp(cls)})(?![-/])`,
            'g',
          ),
          `$1${self.prefix}$2`,
        );
      });
      self.prefixed = code;
    },
  },
  created(){

  }
})
</script>

<style>

.q-field__control {
  height: 100% !important;
}

</style>