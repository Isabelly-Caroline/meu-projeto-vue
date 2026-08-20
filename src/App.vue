<script setup lang="ts">
import { ref, computed } from 'vue'

// Tab state
const activeTab = ref('intro')

// Reactive Playground State
const counter = ref(0)
const textInput = ref('')
const selectedColor = ref('#42b883')

// To-Do list state
const newTodoText = ref('')
const todos = ref([
  { id: 1, text: 'Aprender os conceitos básicos de Vue.js', done: true },
  { id: 2, text: 'Criar um projeto incrível com Vite', done: false },
  { id: 3, text: 'Dominar a Composition API (ref, computed)', done: false },
])

const addTodo = () => {
  if (newTodoText.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: newTodoText.value.trim(),
      done: false,
    })
    newTodoText.value = ''
  }
}

const toggleTodo = (id: number) => {
  const todo = todos.value.find((t) => t.id === id)
  if (todo) todo.done = !todo.done
}

const deleteTodo = (id: number) => {
  todos.value = todos.value.filter((t) => t.id !== id)
}

// Mini-Quiz State
const quizAnswers = ref<{ [key: number]: string }>({})
const quizSubmitted = ref(false)

const quizQuestions = [
  {
    id: 1,
    question: '1. Qual diretiva do Vue é usada para realizar ligação de dados bidirecional (two-way data binding)?',
    options: ['v-bind', 'v-model', 'v-on', 'v-for'],
    correct: 'v-model',
    explanation: 'O v-model cria uma vinculação de dados bidirecional entre um elemento de entrada de formulário (como input ou textarea) e um estado reativo.',
  },
  {
    id: 2,
    question: '2. Qual função da Composition API é usada para criar uma referência reativa para qualquer tipo de dado?',
    options: ['reactive', 'useState', 'ref', 'computed'],
    correct: 'ref',
    explanation: 'A função ref() aceita um valor interno e retorna um objeto de referência reativo e mutável.',
  },
  {
    id: 3,
    question: '3. No ecossistema Vue, qual é a ferramenta oficial e moderna de build recomendada para criar novos projetos?',
    options: ['Webpack', 'Vite', 'Turbopack', 'Parcel'],
    correct: 'Vite',
    explanation: 'O Vite foi criado por Evan You (criador do Vue) e é a ferramenta de build extremamente rápida recomendada para o Vue moderno.',
  },
]

const score = computed(() => {
  let count = 0
  quizQuestions.forEach((q) => {
    if (quizAnswers.value[q.id] === q.correct) {
      count++
    }
  })
  return count
})

const submitQuiz = () => {
  if (Object.keys(quizAnswers.value).length < quizQuestions.length) {
    alert('Por favor, responda todas as perguntas do quiz antes de enviar!')
    return
  }
  quizSubmitted.value = true
}

const resetQuiz = () => {
  quizAnswers.value = {}
  quizSubmitted.value = false
}
</script>

<template>
  <div class="page-container">
    <!-- Header/Hero Section -->
    <header class="hero-section">
      <div class="hero-content">
        <div class="vue-logo-container">
          <svg class="vue-logo" viewBox="0 0 256 221" width="100" height="86" xmlns="http://www.w3.org/2000/svg" preserveAspectRatio="xMidYMid">
            <path fill="#41B883" d="M204.8 0H256L128 220.8L0 0h97.9L128 51.4L157.1 0h47.7z"/>
            <path fill="#35495E" d="M0 0l128 220.8L256 0h-51.2L128 132.5L50.2 0H0z"/>
          </svg>
        </div>
        <h1 class="hero-title">Explorando o Universo <span>Vue.js</span></h1>
        <p class="hero-subtitle">
          Um guia interativo e prático desenvolvido por <strong>Isabelly Caroline</strong>.
        </p>
        <p class="hero-desc">
          Entenda como funciona o framework JavaScript progressivo de forma visual e divertida.
        </p>
      </div>
    </header>

    <!-- Navigation Tabs -->
    <nav class="tab-navigation">
      <button 
        :class="['tab-btn', { active: activeTab === 'intro' }]" 
        @click="activeTab = 'intro'"
      >
        🌟 O que é o Vue?
      </button>
      <button 
        :class="['tab-btn', { active: activeTab === 'comparison' }]" 
        @click="activeTab = 'comparison'"
      >
        🔄 Vue vs React
      </button>
      <button 
        :class="['tab-btn', { active: activeTab === 'playground' }]" 
        @click="activeTab = 'playground'"
      >
        ⚡ Playground Reativo
      </button>
      <button 
        :class="['tab-btn', { active: activeTab === 'quiz' }]" 
        @click="activeTab = 'quiz'"
      >
        🧠 Teste Seus Conhecimentos
      </button>
    </nav>

    <!-- Main Content Area -->
    <main class="main-content">
      <!-- Tab 1: Intro -->
      <section v-if="activeTab === 'intro'" class="tab-pane fade-in">
        <div class="card card-hero">
          <h2>O Framework Progressivo</h2>
          <p>
            O Vue (pronunciado /vjuː/, como "view") é um framework JavaScript para a construção de interfaces de usuário. 
            Ele foi projetado para ser <strong>adotável progressivamente</strong>. Isso significa que se você tem um projeto existente, 
            você pode incluir o Vue em apenas uma parte dele sem precisar reescrever tudo.
          </p>
          <div class="features-grid">
            <div class="feature-card">
              <div class="feature-icon">⚡</div>
              <h3>Fácil Aprendizado</h3>
              <p>Se você já conhece HTML, CSS e JavaScript básico, já consegue começar a criar apps incríveis com Vue rapidamente.</p>
            </div>
            <div class="feature-card">
              <div class="feature-icon">🔋</div>
              <h3>Desempenho Veloz</h3>
              <p>Com um DOM Virtual inteligente e reatividade nativa e otimizada, o Vue entrega uma renderização extremamente rápida.</p>
            </div>
            <div class="feature-card">
              <div class="feature-icon">🧩</div>
              <h3>Componentização</h3>
              <p>Divida sua interface em blocos autônomos, reutilizáveis e fáceis de gerenciar (Arquivos .vue).</p>
            </div>
          </div>
        </div>

        <div class="card code-explanation-card">
          <h2>Como se parece um Componente Vue?</h2>
          <p>No Vue, usamos comumente os chamados <strong>Single File Components (SFC)</strong>, que unem marcação, estilo e lógica em um único arquivo:</p>
          <pre class="code-block"><code>&lt;script setup&gt;
import { ref } from 'vue'
const mensagem = ref('Olá, Isabelly!')
&lt;/script&gt;

&lt;template&gt;
  &lt;div class="box"&gt;
    &lt;h1&gt;&#123;&#123; mensagem &#125;&#125;&lt;/h1&gt;
    &lt;input v-model="mensagem" /&gt;
  &lt;/div&gt;
&lt;/template&gt;

&lt;style scoped&gt;
.box { background: #f0f0f0; padding: 20px; }
&lt;/style&gt;</code></pre>
        </div>
      </section>

      <!-- Tab 2: Comparison (React vs Vue) -->
      <section v-if="activeTab === 'comparison'" class="tab-pane fade-in">
        <div class="card">
          <h2>Vue.js vs React.js</h2>
          <p>
            Embora ambos os frameworks resolvam problemas semelhantes e usem o conceito de Componentes, 
            eles possuem filosofias de design bem diferentes. Veja a comparação detalhada:
          </p>
          
          <div class="table-responsive">
            <table class="comparison-table">
              <thead>
                <tr>
                  <th>Recurso</th>
                  <th>Vue.js (Framework Progressivo)</th>
                  <th>React.js (Biblioteca UI)</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><strong>Estilo de Código</strong></td>
                  <td>Arquivos <code>.vue</code> (SFC) com seções separadas de Script, Template e Style.</td>
                  <td>Geralmente usa JSX (HTML misturado com JavaScript em funções).</td>
                </tr>
                <tr>
                  <td><strong>Gerenciamento de Estado</strong></td>
                  <td>Sistema de Reatividade nativo com <code>ref()</code> e <code>reactive()</code>. Atualizações automáticas.</td>
                  <td>Imutabilidade estrita através de hooks como <code>useState</code>. Atualiza por re-renderização total da função.</td>
                </tr>
                <tr>
                  <td><strong>Curva de Aprendizado</strong></td>
                  <td>Suave. Mais próximo do HTML/CSS clássico. Diretivas intuitivas como <code>v-if</code> e <code>v-for</code>.</td>
                  <td>Média. Requer forte domínio de conceitos modernos de JS (ES6+), mapeamento de arrays e lógica de renderização pura.</td>
                </tr>
                <tr>
                  <td><strong>CSS Integrado</strong></td>
                  <td>Suporte nativo a estilos escopados (<code>&lt;style scoped&gt;</code>) integrados.</td>
                  <td>Necessita de bibliotecas adicionais (CSS Modules, Styled Components, etc.) para isolamento de estilos.</td>
                </tr>
                <tr>
                  <td><strong>Ligação de Dados</strong></td>
                  <td>Bidirecional (Two-way data binding) opcional e simples usando <code>v-model</code>.</td>
                  <td>Unidirecional (One-way data binding). Exige escrever manipuladores de eventos manuais (ex: <code>onChange</code>).</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="comparison-footer-note">
            <p>💡 <strong>Nota da Isabelly:</strong> O React é ótimo para quem quer JavaScript puro em tudo, enquanto o Vue brilha pela elegância, organização e simplicidade no dia a dia do desenvolvimento.</p>
          </div>
        </div>
      </section>

      <!-- Tab 3: Interactive Playground -->
      <section v-if="activeTab === 'playground'" class="tab-pane fade-in">
        <div class="grid-two-columns">
          
          <!-- Column 1: Interactive Elements -->
          <div class="card">
            <h2>⚡ Experimente a Reatividade</h2>
            <p class="section-desc">Interaja com os controles abaixo para ver como o Vue atualiza o DOM instantaneamente.</p>
            
            <!-- Play Item 1: Counter -->
            <div class="playground-item">
              <label class="item-title">Exemplo 1: Contador Reativo (ref)</label>
              <div class="counter-control">
                <button class="btn-circle" @click="counter--" aria-label="Diminuir">-</button>
                <span class="counter-value" :style="{ color: counter >= 0 ? '#42b883' : '#e06c75' }">{{ counter }}</span>
                <button class="btn-circle" @click="counter++" aria-label="Aumentar">+</button>
                <button class="btn btn-secondary" @click="counter = 0">Resetar</button>
              </div>
            </div>

            <!-- Play Item 2: Input Binding & Color Picker -->
            <div class="playground-item">
              <label class="item-title">Exemplo 2: Two-Way Data Binding (v-model)</label>
              <div class="form-group">
                <input 
                  type="text" 
                  v-model="textInput" 
                  placeholder="Escreva algo aqui..." 
                  class="text-input"
                />
              </div>
              <div class="form-group color-picker-group">
                <label for="color-picker">Escolha uma cor para o texto:</label>
                <input 
                  id="color-picker"
                  type="color" 
                  v-model="selectedColor" 
                  class="color-picker"
                />
              </div>
              <div class="live-preview-box" :style="{ borderColor: selectedColor }">
                <p class="preview-label">Visualização em Tempo Real:</p>
                <p class="preview-content" :style="{ color: selectedColor }">
                  {{ textInput || 'Digite algo acima para ver a mágica da reatividade!' }}
                </p>
              </div>
            </div>
          </div>

          <!-- Column 2: Reactive To-Do List -->
          <div class="card">
            <h2>📝 Lista Dinâmica (List Rendering)</h2>
            <p class="section-desc">Demonstração de renderização de listas no Vue usando a diretiva <code>v-for</code>.</p>
            
            <form @submit.prevent="addTodo" class="todo-form">
              <input 
                type="text" 
                v-model="newTodoText" 
                placeholder="Nova tarefa..." 
                class="text-input todo-input"
              />
              <button type="submit" class="btn btn-primary">Adicionar</button>
            </form>

            <ul class="todo-list">
              <li 
                v-for="todo in todos" 
                :key="todo.id" 
                :class="['todo-item', { completed: todo.done }]"
              >
                <span @click="toggleTodo(todo.id)" class="todo-text">
                  <span class="todo-checkbox">
                    {{ todo.done ? '✓' : '' }}
                  </span>
                  {{ todo.text }}
                </span>
                <button class="btn-delete" @click="deleteTodo(todo.id)" aria-label="Excluir tarefa">×</button>
              </li>
            </ul>

            <div class="todo-stats">
              <span>Total: {{ todos.length }} | Pendentes: {{ todos.filter(t => !t.done).length }}</span>
            </div>
          </div>
        </div>
      </section>

      <!-- Tab 4: Quiz -->
      <section v-if="activeTab === 'quiz'" class="tab-pane fade-in">
        <div class="card quiz-card">
          <h2>🧠 Teste Seus Conhecimentos em Vue.js</h2>
          <p class="quiz-intro">Responda a este rápido quiz sobre os conceitos que abordamos e veja seu resultado.</p>
          
          <div v-if="!quizSubmitted" class="quiz-questions-list">
            <div 
              v-for="question in quizQuestions" 
              :key="question.id" 
              class="quiz-question-item"
            >
              <h3>{{ question.question }}</h3>
              <div class="quiz-options">
                <label 
                  v-for="option in question.options" 
                  :key="option" 
                  :class="['quiz-option-label', { selected: quizAnswers[question.id] === option }]"
                >
                  <input 
                    type="radio" 
                    :name="'question-' + question.id" 
                    :value="option" 
                    v-model="quizAnswers[question.id]" 
                    class="radio-input"
                  />
                  {{ option }}
                </label>
              </div>
            </div>
            
            <div class="quiz-actions">
              <button class="btn btn-primary btn-large" @click="submitQuiz">
                Enviar Respostas
              </button>
            </div>
          </div>

          <!-- Quiz Results Screen -->
          <div v-else class="quiz-results fade-in">
            <div class="results-header">
              <div class="score-circle">
                <span class="score-number">{{ score }}</span>
                <span class="score-total">/ {{ quizQuestions.length }}</span>
              </div>
              <h3>
                {{ score === quizQuestions.length ? 'Perfeito! 🌟 Você domina o Vue!' : score >= 1 ? 'Bom trabalho! 👍 Continue estudando!' : 'Tente novamente! 📚 O aprendizado é uma jornada!' }}
              </h3>
            </div>

            <div class="quiz-review-list">
              <div 
                v-for="q in quizQuestions" 
                :key="q.id" 
                :class="['review-item', quizAnswers[q.id] === q.correct ? 'correct-answer' : 'wrong-answer']"
              >
                <h4>{{ q.question }}</h4>
                <p class="review-status">
                  <strong>Sua Resposta:</strong> {{ quizAnswers[q.id] }} 
                  <span v-if="quizAnswers[q.id] === q.correct" class="badge badge-success">Correto!</span>
                  <span v-else class="badge badge-error">Incorreto (Correto: {{ q.correct }})</span>
                </p>
                <p class="review-explanation">
                  <strong>Explicação:</strong> {{ q.explanation }}
                </p>
              </div>
            </div>

            <div class="quiz-actions">
              <button class="btn btn-secondary" @click="resetQuiz">
                Refazer Quiz
              </button>
            </div>
          </div>
        </div>
      </section>
    </main>

    <!-- Footer -->
    <footer class="page-footer">
      <p>Desenvolvido com carinho e 💚 por <strong>Isabelly Caroline</strong></p>
      <p class="footer-links">
        <a href="https://vuejs.org/" target="_blank" rel="noopener">Documentação Oficial do Vue</a>
        <span>•</span>
        <a href="https://github.com/vuejs" target="_blank" rel="noopener">GitHub do Vue</a>
      </p>
      <p class="footer-year">© 2026 - Todos os direitos reservados</p>
    </footer>
  </div>
</template>

<style scoped>
/* Page Container & Layout */
.page-container {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  gap: 2rem;
  color: var(--color-text);
  font-family: inherit;
  width: 100%;
}

/* Animations */
.fade-in {
  animation: fadeIn 0.4s ease-in-out forwards;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Hero Section */
.hero-section {
  text-align: center;
  padding: 3rem 1.5rem;
  background: linear-gradient(135deg, rgba(66, 184, 131, 0.08) 0%, rgba(53, 73, 94, 0.05) 100%);
  border-radius: 16px;
  border: 1px solid rgba(66, 184, 131, 0.15);
  box-shadow: 0 4px 30px rgba(0, 0, 0, 0.02);
}

.vue-logo-container {
  display: flex;
  justify-content: center;
  margin-bottom: 1.5rem;
}

.vue-logo {
  filter: drop-shadow(0 4px 10px rgba(66, 184, 131, 0.2));
  transition: transform 0.6s ease;
}

.vue-logo:hover {
  transform: rotateY(180deg);
}

.hero-title {
  font-size: 2.5rem;
  font-weight: 800;
  color: var(--color-heading);
  margin-bottom: 0.5rem;
  line-height: 1.2;
}

.hero-title span {
  color: #42b883;
  position: relative;
  display: inline-block;
}

.hero-subtitle {
  font-size: 1.2rem;
  color: var(--color-text);
  margin-bottom: 0.75rem;
}

.hero-subtitle strong {
  color: #42b883;
  font-weight: 600;
}

.hero-desc {
  font-size: 0.95rem;
  opacity: 0.8;
  max-width: 600px;
  margin: 0 auto;
}

/* Tabs Navigation */
.tab-navigation {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 0.5rem;
  border-bottom: 2px solid var(--color-border);
  padding-bottom: 0.5rem;
}

.tab-btn {
  background: transparent;
  border: none;
  padding: 0.75rem 1.25rem;
  font-size: 1rem;
  font-weight: 600;
  color: var(--color-text);
  cursor: pointer;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.tab-btn:hover {
  background-color: rgba(66, 184, 131, 0.1);
  color: #42b883;
}

.tab-btn.active {
  background-color: #42b883;
  color: white;
}

/* Cards & Components Styling */
.card {
  background-color: var(--color-background-soft);
  border-radius: 12px;
  border: 1px solid var(--color-border);
  padding: 2rem;
  margin-bottom: 1.5rem;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.03);
}

.card h2 {
  font-size: 1.6rem;
  font-weight: 700;
  color: var(--color-heading);
  margin-bottom: 1rem;
  border-bottom: 2px solid rgba(66, 184, 131, 0.2);
  padding-bottom: 0.5rem;
}

.card p {
  line-height: 1.6;
  margin-bottom: 1rem;
}

.section-desc {
  opacity: 0.85;
  margin-bottom: 1.5rem;
}

/* Features Grid */
.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
  margin-top: 2rem;
}

.feature-card {
  background-color: var(--color-background-mute);
  padding: 1.5rem;
  border-radius: 10px;
  border: 1px solid var(--color-border);
  transition: transform 0.2s ease, border-color 0.2s ease;
}

.feature-card:hover {
  transform: translateY(-4px);
  border-color: #42b883;
}

.feature-icon {
  font-size: 2rem;
  margin-bottom: 0.75rem;
}

.feature-card h3 {
  font-size: 1.2rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  color: var(--color-heading);
}

.feature-card p {
  font-size: 0.9rem;
  opacity: 0.8;
  margin-bottom: 0;
}

/* Code Block styling */
.code-block {
  background-color: #282c34;
  color: #abb2bf;
  padding: 1.25rem;
  border-radius: 8px;
  overflow-x: auto;
  font-family: 'Courier New', Courier, monospace;
  font-size: 0.9rem;
  line-height: 1.4;
  border: 1px solid #1e2127;
}

/* Comparison Table */
.table-responsive {
  width: 100%;
  overflow-x: auto;
  margin-top: 1.5rem;
  border-radius: 8px;
  border: 1px solid var(--color-border);
}

.comparison-table {
  width: 100%;
  border-collapse: collapse;
  text-align: left;
  min-width: 600px;
}

.comparison-table th, 
.comparison-table td {
  padding: 1rem;
  border-bottom: 1px solid var(--color-border);
}

.comparison-table th {
  background-color: rgba(66, 184, 131, 0.1);
  color: var(--color-heading);
  font-weight: 700;
}

.comparison-table tr:hover {
  background-color: rgba(66, 184, 131, 0.02);
}

.comparison-table code {
  background-color: var(--color-background-mute);
  padding: 0.2rem 0.4rem;
  border-radius: 4px;
  font-size: 0.85rem;
  color: #e06c75;
}

.comparison-footer-note {
  margin-top: 1.5rem;
  padding: 1rem;
  border-left: 4px solid #42b883;
  background-color: rgba(66, 184, 131, 0.05);
  border-radius: 0 8px 8px 0;
}

.comparison-footer-note p {
  margin: 0;
}

/* Two Columns Grid for Playground */
.grid-two-columns {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1.5rem;
}

@media (min-width: 768px) {
  .grid-two-columns {
    grid-template-columns: 1fr 1fr;
  }
}

/* Form & Inputs */
.form-group {
  margin-bottom: 1rem;
}

.text-input {
  width: 100%;
  padding: 0.75rem 1rem;
  font-size: 0.95rem;
  border-radius: 8px;
  border: 1px solid var(--color-border);
  background-color: var(--color-background);
  color: var(--color-text);
  outline: none;
  transition: border-color 0.2s ease;
}

.text-input:focus {
  border-color: #42b883;
}

.color-picker-group {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-top: 0.5rem;
}

.color-picker {
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  background: none;
}

/* Interactive Playground Elements */
.playground-item {
  margin-bottom: 1.5rem;
  padding-bottom: 1.5rem;
  border-bottom: 1px dashed var(--color-border);
}

.playground-item:last-child {
  border-bottom: none;
  margin-bottom: 0;
  padding-bottom: 0;
}

.item-title {
  display: block;
  font-weight: 600;
  margin-bottom: 0.75rem;
  color: var(--color-heading);
}

.counter-control {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.counter-value {
  font-size: 1.8rem;
  font-weight: 700;
  min-width: 50px;
  text-align: center;
}

.live-preview-box {
  margin-top: 1rem;
  padding: 1rem;
  border-radius: 8px;
  border: 2px dashed #42b883;
  background-color: var(--color-background-mute);
}

.preview-label {
  font-size: 0.8rem;
  opacity: 0.6;
  margin-bottom: 0.25rem !important;
}

.preview-content {
  font-size: 1.1rem;
  font-weight: 600;
  word-break: break-all;
  margin-bottom: 0 !important;
}

/* Buttons */
.btn {
  padding: 0.6rem 1.2rem;
  font-size: 0.95rem;
  font-weight: 600;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s ease;
  border: none;
}

.btn-circle {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #42b883;
  color: white;
  border: none;
  font-size: 1.5rem;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.btn-circle:hover {
  background-color: #35495e;
}

.btn-primary {
  background-color: #42b883;
  color: white;
}

.btn-primary:hover {
  background-color: #35495e;
}

.btn-secondary {
  background-color: var(--color-background-mute);
  color: var(--color-text);
  border: 1px solid var(--color-border);
}

.btn-secondary:hover {
  background-color: var(--color-border);
}

.btn-large {
  padding: 0.8rem 2rem;
  font-size: 1.1rem;
}

/* To-Do list styling */
.todo-form {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.todo-input {
  flex: 1;
}

.todo-list {
  list-style: none;
  padding: 0;
  margin: 0 0 1rem 0;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem 1rem;
  background-color: var(--color-background);
  border: 1px solid var(--color-border);
  border-radius: 8px;
  transition: all 0.2s ease;
}

.todo-item.completed {
  border-color: rgba(66, 184, 131, 0.3);
  background-color: rgba(66, 184, 131, 0.02);
}

.todo-item.completed .todo-text {
  text-decoration: line-through;
  opacity: 0.6;
}

.todo-text {
  flex: 1;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  user-select: none;
}

.todo-checkbox {
  width: 20px;
  height: 20px;
  border-radius: 4px;
  border: 1px solid var(--color-border);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.8rem;
  font-weight: bold;
  color: #42b883;
  background-color: var(--color-background-soft);
}

.todo-item.completed .todo-checkbox {
  background-color: rgba(66, 184, 131, 0.1);
  border-color: #42b883;
}

.btn-delete {
  background: transparent;
  border: none;
  font-size: 1.4rem;
  color: #e06c75;
  cursor: pointer;
  opacity: 0.7;
  padding: 0 0.5rem;
}

.btn-delete:hover {
  opacity: 1;
}

.todo-stats {
  font-size: 0.85rem;
  opacity: 0.7;
  text-align: right;
}

/* Quiz styling */
.quiz-card {
  max-width: 800px;
  margin: 0 auto;
}

.quiz-intro {
  margin-bottom: 2rem !important;
}

.quiz-question-item {
  margin-bottom: 2rem;
  padding-bottom: 1.5rem;
  border-bottom: 1px solid var(--color-border);
}

.quiz-question-item h3 {
  font-size: 1.15rem;
  font-weight: 600;
  margin-bottom: 1rem;
  color: var(--color-heading);
}

.quiz-options {
  display: grid;
  grid-template-columns: 1fr;
  gap: 0.75rem;
}

@media (min-width: 600px) {
  .quiz-options {
    grid-template-columns: 1fr 1fr;
  }
}

.quiz-option-label {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1rem;
  border: 1px solid var(--color-border);
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s ease;
  background-color: var(--color-background);
  user-select: none;
}

.quiz-option-label:hover {
  border-color: #42b883;
  background-color: rgba(66, 184, 131, 0.02);
}

.quiz-option-label.selected {
  border-color: #42b883;
  background-color: rgba(66, 184, 131, 0.08);
  font-weight: 600;
}

.radio-input {
  accent-color: #42b883;
}

.quiz-actions {
  display: flex;
  justify-content: center;
  margin-top: 2rem;
}

.quiz-results {
  text-align: center;
  padding: 1rem 0;
}

.results-header {
  margin-bottom: 2rem;
}

.score-circle {
  display: inline-flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 120px;
  height: 120px;
  border-radius: 50%;
  background: linear-gradient(135deg, #42b883 0%, #35495e 100%);
  color: white;
  margin-bottom: 1.5rem;
  box-shadow: 0 8px 24px rgba(66, 184, 131, 0.3);
}

.score-number {
  font-size: 2.5rem;
  font-weight: 800;
  line-height: 1;
}

.score-total {
  font-size: 1rem;
  opacity: 0.8;
}

.quiz-review-list {
  text-align: left;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.review-item {
  padding: 1.25rem;
  border-radius: 8px;
  border: 1px solid var(--color-border);
}

.review-item h4 {
  font-size: 1rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.review-status {
  margin-bottom: 0.5rem !important;
}

.badge {
  display: inline-block;
  padding: 0.15rem 0.5rem;
  font-size: 0.75rem;
  font-weight: bold;
  border-radius: 4px;
  margin-left: 0.5rem;
}

.badge-success {
  background-color: rgba(66, 184, 131, 0.2);
  color: #42b883;
}

.badge-error {
  background-color: rgba(224, 108, 117, 0.2);
  color: #e06c75;
}

.review-explanation {
  font-size: 0.9rem;
  opacity: 0.85;
  background-color: var(--color-background);
  padding: 0.75rem;
  border-radius: 6px;
  border-left: 3px solid #35495e;
  margin-bottom: 0 !important;
}

.correct-answer {
  border-left: 4px solid #42b883;
}

.wrong-answer {
  border-left: 4px solid #e06c75;
}

/* Footer styling */
.page-footer {
  margin-top: auto;
  text-align: center;
  padding: 2.5rem 1rem;
  border-top: 1px solid var(--color-border);
  font-size: 0.9rem;
  color: var(--color-text);
  opacity: 0.9;
}

.page-footer p {
  margin: 0 0 0.5rem 0;
}

.footer-links {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 0.5rem !important;
}

.footer-links a {
  font-weight: 600;
}

.footer-year {
  font-size: 0.8rem;
  opacity: 0.6;
}
</style>
