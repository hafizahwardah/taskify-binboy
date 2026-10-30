
<script setup>
import { ref } from 'vue'

//states: buat menyimpan data tugas atau tasks
const tasks = ref([])

//states:buat menyimpan inputan dari form
const inputTask = ref('')

//action: buat menambahkan tugas baru ke dalam daftar tasks
const addTask = (title) => {

  //validasi agar tugas kosong tidak bisa ditambahkan
  if (!title.trim()) return

  //bikin objeck task baru
  const newTask = {
    id: new Date().getTime(),
    title: title.trim(),
    status: false
  }

  tasks.value.push(newTask)

  inputTask.value = ''
}

//action
const deleteTask = (id) => {

  const task = tasks.value.find(item => item.id === id)

  if (!task) {
    alert("Task tidak ditemukan")
    return
  }

  //harus menyelesaikan tugas sebelum menghapus
  if (!task.status) {
    alert("Centang tugas terlebih dahulu sebelum menghapus!")
    return
  }

  //minta konfirmasi ke user
  const confirmation = confirm("Apakah anda yakin ingin menghapus?")

  if (confirmation) {
    //hapus task dari state tasks
    tasks.value = tasks.value.filter(item => item.id !== id)
  }
}
</script>

<template>
  <section
    class="relative isolate min-h-screen overflow-hidden bg-[#fff8f0] px-4 py-8 sm:px-6"
  >

    <!-- dekorasi background orange -->
    <div
      class="pointer-events-none absolute -left-16 -top-16 -z-10 h-56 w-72 rounded-br-[150px] bg-gradient-to-br from-orange-300 to-orange-500"
    ></div>

    <div
      class="pointer-events-none absolute -bottom-24 -right-16 -z-10 h-64 w-80 rounded-tl-[150px] bg-gradient-to-br from-orange-300 to-orange-500"
    ></div>

    <div class="mx-auto w-full max-w-6xl">

      <!-- judul aplikasi -->
      <header class="mb-9 text-center">

        <div
          class="mx-auto mb-3 flex h-16 w-16 items-center justify-center rounded-[21px] bg-gradient-to-br from-orange-400 to-orange-600 text-4xl font-extrabold text-white shadow-[7px_7px_18px_#edc6a2,-6px_-6px_16px_#ffffff]"
        >
          ✓
        </div>

        <h1
          class="text-4xl font-extrabold tracking-tight text-[#603719] sm:text-5xl"
        >
          Taskify Vue
        </h1>

        <p class="mt-2 text-base text-[#b66a32] sm:text-lg">
          yuk, selesaikan tugasmu hari ini!
        </p>

      </header>

      <!-- formulir tambah tugas -->
      <div
        class="mb-6 rounded-[27px] border border-orange-100 bg-[#fff8f0] p-3 shadow-[9px_9px_24px_#f2dcc8,-8px_-8px_22px_#ffffff]"
      >

        <form
          @submit.prevent="addTask(inputTask)"
          action=""
          class="flex flex-col gap-3 sm:flex-row"
        >

          <div class="relative min-w-0 flex-1">

            <span
              class="pointer-events-none absolute left-5 top-1/2 -translate-y-1/2 text-2xl text-orange-500"
            >
              ✎
            </span>

            <input
              v-model="inputTask"
              type="text"
              placeholder="Masukkan tugas baru"
              aria-label="Tugas baru"
              class="w-full rounded-[22px] border-2 border-[#f7dfcd] bg-[#fffaf5] py-4 pl-14 pr-4 text-base text-[#603719] outline-none transition placeholder:text-[#ad9583] focus:border-orange-300 focus:ring-4 focus:ring-orange-100"
            />

          </div>

          <button
            type="submit"
            :disabled="!inputTask.trim()"
            class="flex min-h-14 items-center justify-center gap-2 rounded-[22px] bg-gradient-to-r from-orange-400 to-orange-600 px-7 py-3 font-bold text-white shadow-[6px_7px_15px_#f0c6a2] transition hover:-translate-y-0.5 hover:shadow-lg disabled:cursor-not-allowed disabled:opacity-40 disabled:shadow-none"
          >
            <span class="text-2xl">＋</span>
            Tambah
          </button>

        </form>

      </div>

      <!-- daftar tugas -->
      <ul
        class="flex flex-col gap-4 rounded-[30px] border border-white bg-[#fff8f0] p-3 shadow-[10px_10px_28px_#f0ddca,-9px_-9px_24px_#ffffff] sm:p-5"
        v-auto-animate
      >

        <li
          v-for="task in tasks"
          :key="task.id"
          class="flex min-h-[67px] items-center justify-between gap-3 rounded-[22px] border border-orange-50 bg-[#fffaf5] px-3 py-4 shadow-[5px_5px_13px_#f3e3d4,-5px_-5px_13px_#ffffff] transition hover:-translate-y-0.5 sm:px-5"
          :class="task.status ? 'bg-[#fff0df]' : ''"
        >

          <label
            class="flex min-w-0 flex-1 cursor-pointer items-center gap-3 sm:gap-4"
          >

            <input
              v-model="task.status"
              type="checkbox"
              class="peer sr-only"
            />

            <span
              class="flex h-7 w-7 shrink-0 items-center justify-center rounded-[9px] border-2 border-orange-400 bg-[#fffaf5] text-lg font-extrabold text-white transition peer-focus-visible:ring-4 peer-focus-visible:ring-orange-200 sm:h-8 sm:w-8"
              :class="task.status ? 'border-orange-500 bg-gradient-to-br from-orange-400 to-orange-600 shadow-md' : ''"
            >
              <span v-if="task.status">✓</span>
            </span>

            <span
              class="break-words text-base font-semibold text-[#603719] transition sm:text-lg"
              :class="task.status ? 'text-[#b77a4c] line-through decoration-2' : ''"
            >
              {{ task.title }}
            </span>

          </label>

          <button
            @click="deleteTask(task.id)"
            :disabled="!task.status"
            class="flex shrink-0 items-center justify-center gap-2 rounded-full border border-orange-100 bg-[#fff0e2] px-3 py-2.5 text-sm font-bold text-orange-700 transition hover:bg-orange-100 disabled:cursor-not-allowed disabled:opacity-40 sm:px-4"
          >
            <span>▤</span>
            Hapus
          </button>

        </li>

      </ul>

      <!-- tampilan jika belum ada tugas -->
      <div
        v-if="tasks.length === 0"
        class="mt-5 rounded-3xl bg-[#fffaf5] px-5 py-8 text-center text-[#a87955] shadow-[6px_6px_16px_#f0ddca,-6px_-6px_16px_#ffffff]"
      >
        <div class="text-3xl text-orange-400">✎</div>
        <p class="mt-2">
          belum ada tugas, yuk tambahkan tugas pertamamu!
        </p>
      </div>

      <!-- jumlah tugas selesai -->
      <p
        v-if="tasks.length > 0"
        class="mt-4 text-right text-sm text-[#a87955]"
      >
        {{ tasks.filter(task => task.status).length }} dari
        {{ tasks.length }} tugas selesai
      </p>

    </div>
  </section>
</template>
