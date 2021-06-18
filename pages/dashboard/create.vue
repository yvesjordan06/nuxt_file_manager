<template>
  <div class="py-8">
    <h2 class="mb-8 text-4xl font-bold">Create a new document</h2>

    <div>
      <!-- component -->
      <div class="sm:px-8 md:px-16 sm:py-8">
        <main class="container h-full max-w-screen-lg mx-auto">
          <!-- file upload modal -->
          <article
            aria-label="File Upload Modal"
            class="relative flex flex-col h-full bg-white rounded-md shadow-xl"
          >
            <!-- scroll area -->
            <section class="flex flex-col w-full h-full p-8 overflow-auto">
              <header
                @drop.prevent="addFile"
                @dragenter.prevent="entered = true"
                @dragleave.prevent="entered = false"
                @dragover.prevent
                v-cloak
                class="flex flex-col items-center justify-center py-12 border-2 border-gray-400 border-dashed "
              >
                <div v-if="!file">
                  <p
                    v-if="!entered"
                    class="flex flex-wrap justify-center mb-3 font-semibold text-gray-900 "
                  >
                    <span>Drag and drop your</span>&nbsp;<span
                      >files anywhere or</span
                    >
                  </p>

                  <p
                    v-else
                    class="flex flex-wrap justify-center mb-3 font-semibold text-gray-900 "
                  >
                    <span>Drop your file to upload</span>
                  </p>

                  <input
                    id="hidden-input"
                    type="file"
                    multiple
                    class="hidden"
                  />
                  <div class="flex justify-center">
                    <button
                      id="button"
                      class="px-3 py-1 mt-2 bg-gray-200 rounded-sm hover:bg-gray-300 focus:shadow-outline focus:outline-none"
                    >
                      Upload a file
                    </button>
                  </div>
                </div>

                <div v-else>
                  <p
                    class="flex flex-wrap justify-center mb-3 font-semibold text-gray-900 "
                  >
                    {{ file.name }}
                  </p>
                  <p class="text-sm text-center text-gray-500">
                    {{ formatByte(file.size) }}
                  </p>
                </div>
              </header>

              <div class="mt-2" v-if="progress">
                <h1 class="pt-8 pb-3 font-semibold text-gray-900 sm:text-lg">
                  Upload Progresss
                </h1>
                <span class="text-sm text-gray-600" v-if="uploading"
                  >Uploading please wait</span
                >

                <div class="w-full shadow bg-grey-light">
                  <div
                    class="py-1 text-xs leading-none text-center text-white bg-green-500 "
                    :style="`width: ${progress}%`"
                  >
                    {{ progressString }}
                  </div>
                </div>
              </div>
            </section>

            <!-- sticky footer -->
            <footer class="flex justify-end px-8 pt-4 pb-8">
              <button
                :class="{
                  '!bg-gray-200 cursor-not-allowed hover:!bg-gray-200 ':
                    !file || progress,
                }"
                id="submit"
                @click.prevent="upload"
                class="px-3 py-1 text-white bg-green-700 rounded-sm hover:bg-green-500 focus:shadow-outline focus:outline-none"
              >
                Upload now
              </button>
            </footer>

            <section>
              <!-- component -->
              <div class="flex flex-col px-8 pt-6 pb-8 my-2 mb-4">
                <div class="mb-6 -mx-3 md:flex">
                  <div class="px-3 mb-6 md:w-1/2 md:mb-0">
                    <label
                      class="block mb-2 text-xs font-bold tracking-wide uppercase text-grey-darker"
                      for="grid-first-name"
                    >
                      File Name
                    </label>
                    <input
                      v-model="doc.title"
                      class="block w-full px-4 py-3 mb-3 border rounded appearance-none bg-grey-lighter text-grey-darker border-red"
                      id="grid-first-name"
                      type="text"
                    />
                    <p class="text-xs italic text-red">
                      Please fill out this field.
                    </p>
                  </div>
                  <div class="px-3 md:w-1/2">
                    <label
                      class="block mb-2 text-xs font-bold tracking-wide uppercase text-grey-darker"
                      for="grid-last-name"
                    >
                      Assigned to
                    </label>
                    <select
                      class="block w-full px-4 py-3 border rounded appearance-none bg-grey-lighter text-grey-darker border-grey-lighter"
                      id="grid-last-name"
                      type="text"
                      placeholder="Doe"
                      v-model.number="doc.receiver_id"
                    >
                      <option
                        v-for="user in users"
                        :key="user.id"
                        :value="user.id"
                      >
                        {{ user.name }}
                      </option>
                    </select>
                  </div>
                </div>
                <div class="mb-6 -mx-3 md:flex">
                  <div class="px-3 md:w-full">
                    <label
                      class="block mb-2 text-xs font-bold tracking-wide uppercase text-grey-darker"
                      for="grid-password"
                    >
                      Object
                    </label>
                    <input
                      class="block w-full px-4 py-3 mb-3 border rounded appearance-none bg-grey-lighter text-grey-darker border-grey-lighter"
                      id="grid-password"
                      type="text"
                      v-model="doc.object"
                    />
                    <p class="text-xs italic text-grey-dark">
                      Make it as long and as crazy as you'd like
                    </p>
                  </div>
                </div>

                <div class="mb-6 -mx-3 md:flex">
                  <div class="px-3 md:w-full">
                    <label
                      class="block mb-2 text-xs font-bold tracking-wide uppercase text-grey-darker"
                      for="grid-password"
                    >
                      Reference
                    </label>
                    <textarea
                      v-model="doc.reference"
                      class="block w-full px-4 py-3 mb-3 border rounded appearance-none bg-grey-lighter text-grey-darker border-grey-lighter"
                      id="grid-password"
                    ></textarea>
                    <p class="text-xs italic text-grey-dark">
                      Make it as long and as crazy as you'd like
                    </p>
                  </div>
                </div>
              </div>
            </section>
            <div class="flex justify-end p-8">
              <button
                @click="createDocument"
                class="p-2 px-4 text-center text-white bg-green-700 rounded-lg hover:bg-green-500"
              >
                Submit
              </button>
            </div>
          </article>
        </main>
      </div>
    </div>
  </div>
</template>

<script>
import { formatBytes } from '../../utilities/convertsize'
export default {
  layout: 'dashboard',

  async fetch() {
    this.users = await this.$axios.$get('/users')
    if (this.users) {
      this.doc.receiver_id = this.users[0].id
    }
  },
  data() {
    return {
      file: null,
      fileData: null,
      entered: false,
      progress: 0,
      users: [],
      uploading: false,
      formatByte: formatBytes,
      doc: {
        title: '',
        reference: '',
        object: '',
        file_id: '',
        receiver_id: '',
      },
    }
  },

  methods: {
    addFile(e) {
      this.entered = false

      let droppedFiles = e.dataTransfer.files
      if (!droppedFiles)
        return // this tip, convert FileList to array, credit: https://www.smashingmagazine.com/2018/01/drag-drop-file-uploader-vanilla-js/
      ;[...droppedFiles].forEach((f) => {
        this.file = f
      })
      if (this.doc.title.trim() === '') {
        this.doc.title = this.file.name
      }
      console.log(this.file)
    },
    removeFile(file) {
      this.files = this.files.filter((f) => {
        return f != file
      })
    },
    async createDocument() {

      if (!this.doc.file_id) {
         await this.upload()
      }

      if (!this.doc.title) {
        alert('No title')
        return
      }

      if (!this.doc.receiver_id) {
        alert('No receiver selected')
        return
      }

try {
  await this.$axios.$post('/document', this.doc)
   this.$router.push("/dashboard" )
} catch (error) {
  alert(error)
}



    },

    async upload() {
      if (!this.file || this.uploading) return
      this.uploading = true
      let formData = new FormData()
      formData.append('file', this.file)

      try {
        this.fileData = await this.$axios.$post('/files/upload', formData, {
          onUploadProgress: (progressEvent) => {
            if (progressEvent.lengthComputable) {
              console.log(progressEvent.loaded + ' / ' + progressEvent.total)
              this.progress = (progressEvent.loaded / progressEvent.total) * 100
              this.progressString =
                formatBytes(progressEvent.loaded) +
                ' / ' +
                formatBytes(progressEvent.total)
            }
          },
        })

        this.doc.file_id = this.fileData.ID
      } catch (error) {
        alert(error)
      }

      this.uploading = false
    },
  },
}
</script>

<style  scoped>
.hasImage:hover section {
  background-color: rgba(5, 5, 5, 0.4);
}
.hasImage:hover button:hover {
  background: rgba(5, 5, 5, 0.45);
}

#overlay p,
i {
  opacity: 0;
}

#overlay.draggedover {
  background-color: rgba(255, 255, 255, 0.7);
}
#overlay.draggedover p,
#overlay.draggedover i {
  opacity: 1;
}

.group:hover .group-hover\:text-blue-800 {
  color: #2b6cb0;
}
</style>
