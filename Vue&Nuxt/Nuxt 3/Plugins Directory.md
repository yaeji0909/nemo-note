Only files at the top level of the `plugins/` directory (or index files within any subdirectories) will be registered as plugins.

plugins
 | - myPlugin.ts  // scanned
 | - myOtherPlugin
 | --- supportingFile.ts   // not scanned
 | --- componentToRegister.vue   // not scanned
 | --- index.ts  // currently scanned but deprecated

export default defineNuxtPlugin(nuxtApp => {

// Doing something with nuxtApp

})