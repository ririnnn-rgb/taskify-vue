<script setup>
import { ref, computed } from "vue"

// Variable tasks dengan beberapa data awal
const tasks = ref([
  { id: 1, title: "Finish Figma Wireframes: Client Project", category: "DESIGN", isDone: false },
  { id: 2, title: "Brainstorm: Y2K apparel concept", category: "VIBE", isDone: true },
  { id: 3, title: "Code Sprint: Integrate Taskify backend", category: "DEV", isDone: false },
  { id: 4, title: "Vibe Check 2.0: Portfolio update", category: "LIFE", isDone: true }
])

const taskInput = ref("")
const selectedCategory = ref("DESIGN")

// Function untuk menambah task baru
const addtask = () => {
  if (taskInput.value.trim() === "") return alert("Isi dulu task-nya, fam! ✨")

  const newtask = {
    id: Date.now(),
    title: taskInput.value,
    category: selectedCategory.value,
    isDone: false
  }

  tasks.value.push(newtask)
  taskInput.value = ""
}

// Function untuk menghapus task
const removeTask = (id) => {
  const isConfirmed = confirm("Yakin mau hapus task ini? No cap? 🗑️")
  if (!isConfirmed) return

  const taskindex = tasks.value.findIndex(task => task.id === id)
  if (taskindex !== -1) {
    tasks.value.splice(taskindex, 1)
  }
}

// Menghitung persentase progress
const completedCount = computed(() => tasks.value.filter(t => t.isDone).length)
const progressPercentage = computed(() => {
  if (tasks.value.length === 0) return 0
  return Math.round((completedCount.value / tasks.value.length) * 100)
})
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-pink-100 via-purple-100 to-indigo-100 p-4 sm:p-8 font-sans text-slate-800 flex items-center justify-center">
    
    <!-- Outer Card Wrapper -->
    <div class="w-full max-w-4xl bg-purple-200/60 backdrop-blur-md p-6 sm:p-8 rounded-[2.5rem] border-4 border-white shadow-2xl shadow-purple-300/50">
      
      <!-- Top Navigation / Header -->
      <header class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 mb-8">
        <div class="flex items-center gap-3">
          <h1 class="text-3xl font-black tracking-wider text-slate-800 uppercase flex items-center gap-2">
            TASKIFY <span class="text-xl">⚡</span>
          </h1>
          <span class="bg-teal-200 text-teal-800 text-xs font-extrabold px-3 py-1 rounded-full border-2 border-slate-800 shadow-[2px_2px_0px_#1e293b]">
            (beta 1.0)
          </span>
        </div>

        <!-- User Profile Pill -->
        <div class="flex items-center gap-3 bg-cream-100 bg-amber-50 border-2 border-slate-800 rounded-full px-4 py-1.5 shadow-[3px_3px_0px_#1e293b]">
          <div class="w-8 h-8 rounded-full bg-pink-300 border-2 border-slate-800 flex items-center justify-center text-sm font-bold">
            👾
          </div>
          <span class="font-bold text-sm">@rrrr</span>
          <span class="text-xs bg-pink-200 px-2 py-0.5 rounded-full border border-slate-800 font-semibold">PRO</span>
        </div>
      </header>

      <!-- Main Layout Grid -->
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
        
        <!-- Left Section: Add & Task List (2 cols) -->
        <div class="lg:col-span-2 space-y-6">
          
          <!-- Input Form Card -->
          <div class="bg-amber-50 border-3 border-slate-800 p-5 rounded-3xl shadow-[5px_5px_0px_#1e293b]">
            <h2 class="text-xs font-black uppercase tracking-wider text-slate-500 mb-3">➕ TAMBAH TASK BARU</h2>
            <form @submit.prevent="addtask" class="flex flex-col sm:flex-row gap-2">
              <input 
                v-model="taskInput" 
                type="text" 
                placeholder="Mau ngerjain apa hari ini?..." 
                class="flex-1 bg-white border-2 border-slate-800 rounded-2xl px-4 py-2.5 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-pink-300 transition-all placeholder-slate-400"
              />
              <select 
                v-model="selectedCategory" 
                class="bg-pink-100 border-2 border-slate-800 rounded-2xl px-3 py-2.5 text-xs font-bold focus:outline-none cursor-pointer"
              >
                <option value="DESIGN">DESIGN</option>
                <option value="DEV">DEV</option>
                <option value="VIBE">VIBE</option>
                <option value="LIFE">LIFE</option>
              </select>
              <button 
                type="submit" 
                class="bg-pink-300 hover:bg-pink-400 active:translate-y-0.5 active:shadow-[1px_1px_0px_#1e293b] text-slate-900 font-black px-6 py-2.5 rounded-2xl border-2 border-slate-800 shadow-[3px_3px_0px_#1e293b] text-sm transition-all flex items-center justify-center gap-1"
              >
                ADD IT! ✨
              </button>
            </form>
          </div>

          <!-- Tasks List Container -->
          <div class="bg-blue-50/80 border-3 border-slate-800 p-5 rounded-3xl shadow-[5px_5px_0px_#1e293b] relative">
            
            <!-- Badge Sticker Decor -->
            <div class="absolute -top-4 -right-2 bg-purple-300 text-slate-800 text-[10px] font-black px-3 py-1 rounded-lg border-2 border-slate-800 shadow-[2px_2px_0px_#1e293b] rotate-6">
              Y2K VIBE 💖
            </div>

            <div class="flex items-center justify-between mb-4">
              <h2 class="text-sm font-black uppercase tracking-wider text-slate-700">
                ALL MY STUFF ({{ tasks.length }})
              </h2>
            </div>

            <!-- Empty State -->
            <div v-if="tasks.length === 0" class="text-center py-10 bg-white/50 rounded-2xl border-2 border-dashed border-slate-300">
              <p class="text-sm font-bold text-slate-400">Gak ada task sama sekali. Santai banget! ☕</p>
            </div>

            <!-- Task Items Grid -->
            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
              <div 
                v-for="task in tasks" 
                :key="task.id"
                :class="[
                  task.isDone ? 'bg-slate-100/80 opacity-60' : 'bg-white',
                  'border-2 border-slate-800 p-4 rounded-2xl shadow-[3px_3px_0px_#1e293b] flex flex-col justify-between transition-all group hover:-translate-y-0.5'
                ]"
              >
                <div class="flex items-start gap-3">
                  <input 
                    v-model="task.isDone" 
                    type="checkbox" 
                    class="mt-1 w-5 h-5 rounded-md border-2 border-slate-800 text-pink-400 focus:ring-0 accent-pink-400 cursor-pointer"
                  />
                  <span 
                    :class="task.isDone ? 'line-through text-slate-400' : 'text-slate-800 font-bold'" 
                    class="text-sm leading-snug break-words"
                  >
                    {{ task.title }}
                  </span>
                </div>

                <div class="flex items-center justify-between mt-4 pt-2 border-t border-slate-100">
                  <!-- Tag Category -->
                  <span 
                    :class="{
                      'bg-pink-100 text-pink-700': task.category === 'DESIGN',
                      'bg-blue-100 text-blue-700': task.category === 'DEV',
                      'bg-amber-100 text-amber-700': task.category === 'VIBE',
                      'bg-emerald-100 text-emerald-700': task.category === 'LIFE'
                    }"
                    class="text-[10px] font-black px-2.5 py-0.5 rounded-full border border-slate-800"
                  >
                    {{ task.category }}
                  </span>

                  <!-- Delete Button -->
                  <button 
                    @click="removeTask(task.id)" 
                    class="text-xs text-rose-500 hover:text-rose-700 font-bold hover:underline"
                  >
                    Hapus
                  </button>
                </div>
              </div>
            </div>

          </div>

        </div>

        <!-- Right Section: Progress & Stats (1 col) -->
        <div class="space-y-6">
          
          <!-- Progress Card -->
          <div class="bg-teal-50 border-3 border-slate-800 p-5 rounded-3xl shadow-[5px_5px_0px_#1e293b]">
            <h2 class="text-xs font-black uppercase tracking-wider text-slate-500 mb-2">DAILY PROGRESS 📈</h2>
            
            <div class="flex items-baseline justify-between mb-2">
              <span class="text-2xl font-black text-slate-800">{{ progressPercentage }}%</span>
              <span class="text-xs font-bold text-slate-500">{{ completedCount }} dari {{ tasks.length }} selesai</span>
            </div>

            <!-- Progress Bar -->
            <div class="w-full bg-white border-2 border-slate-800 h-4 rounded-full p-0.5 overflow-hidden">
              <div 
                class="bg-gradient-to-r from-pink-300 via-purple-300 to-teal-300 h-full rounded-full transition-all duration-500 border-r-2 border-slate-800"
                :style="{ width: progressPercentage + '%' }"
              ></div>
            </div>
            
            <p class="text-[11px] font-bold text-slate-500 mt-3 text-center">
              {{ progressPercentage === 100 ? 'Slay! Semua selesai! 🎉' : 'Keep going, slaying it! 💪' }}
            </p>
          </div>

          <!-- Extra Widget / Motivation -->
          <div class="bg-pink-100 border-3 border-slate-800 p-5 rounded-3xl shadow-[5px_5px_0px_#1e293b] text-center">
            <span class="text-3xl block mb-2">✨💅</span>
            <h3 class="font-black text-sm text-slate-800 uppercase">Main Character Energy</h3>
            <p class="text-xs font-medium text-slate-600 mt-1">
              Fokus selesain task satu per satu, jangan lupa stay hydrated!
            </p>
          </div>

        </div>

      </div>

    </div>

  </div>
</template>