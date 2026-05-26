<template>
  <div class="app" :class="{ 'sidebar-collapsed': collapsed }">
    <aside class="sidebar">
      <div class="logo">
        <div class="brand-text">
          <h1>{{ t('nav.companyName') }}</h1>
          <span class="subtitle">{{ t('nav.subtitle') }}</span>
        </div>
        <button
          v-if="!isNarrow"
          class="sidebar-toggle"
          @click="toggleSidebar"
          :aria-label="collapsed ? 'Expand sidebar' : 'Collapse sidebar'"
          :aria-expanded="!collapsed"
        >
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M12 15L7 10L12 5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </button>
      </div>
      <nav class="nav-tabs">
        <router-link
          to="/"
          :class="{ active: $route.path === '/' }"
          :aria-current="$route.path === '/' ? 'page' : undefined"
          :title="t('nav.overview')"
        >
          <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none">
            <rect x="3" y="3" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="11" y="3" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="3" y="11" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="11" y="11" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span class="nav-label">{{ t('nav.overview') }}</span>
        </router-link>
        <router-link
          to="/inventory"
          :class="{ active: $route.path === '/inventory' }"
          :aria-current="$route.path === '/inventory' ? 'page' : undefined"
          :title="t('nav.inventory')"
        >
          <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M10 2.5L17.5 6.5V13.5L10 17.5L2.5 13.5V6.5L10 2.5Z" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <path d="M2.5 6.5L10 10.5L17.5 6.5" stroke="currentColor" stroke-width="1.5" stroke-linejoin="round"/>
            <path d="M10 10.5V17.5" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span class="nav-label">{{ t('nav.inventory') }}</span>
        </router-link>
        <router-link
          to="/orders"
          :class="{ active: $route.path === '/orders' }"
          :aria-current="$route.path === '/orders' ? 'page' : undefined"
          :title="t('nav.orders')"
        >
          <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M7 3H5C4.44772 3 4 3.44772 4 4V16C4 16.5523 4.44772 17 5 17H15C15.5523 17 16 16.5523 16 16V4C16 3.44772 15.5523 3 15 3H13" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            <rect x="7" y="2" width="6" height="3" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <path d="M7 9H13M7 12H11" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="nav-label">{{ t('nav.orders') }}</span>
        </router-link>
        <router-link
          to="/spending"
          :class="{ active: $route.path === '/spending' }"
          :aria-current="$route.path === '/spending' ? 'page' : undefined"
          :title="t('nav.finance')"
        >
          <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none">
            <circle cx="10" cy="10" r="7.5" stroke="currentColor" stroke-width="1.5"/>
            <path d="M10 6V14" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
            <path d="M12.5 8C12.5 7.17 11.38 6.5 10 6.5C8.62 6.5 7.5 7.17 7.5 8C7.5 8.83 8.62 9.5 10 9.5C11.38 9.5 12.5 10.17 12.5 11C12.5 11.83 11.38 12.5 10 12.5C8.62 12.5 7.5 11.83 7.5 11" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="nav-label">{{ t('nav.finance') }}</span>
        </router-link>
        <router-link
          to="/demand"
          :class="{ active: $route.path === '/demand' }"
          :aria-current="$route.path === '/demand' ? 'page' : undefined"
          :title="t('nav.demandForecast')"
        >
          <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M3 15L7 10L10.5 13L15.5 6" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M13 6H15.5V8.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <span class="nav-label">{{ t('nav.demandForecast') }}</span>
        </router-link>
        <router-link
          to="/reports"
          :class="{ active: $route.path === '/reports' }"
          :aria-current="$route.path === '/reports' ? 'page' : undefined"
          :title="t('nav.reports') || 'Reports'"
        >
          <svg class="nav-icon" width="20" height="20" viewBox="0 0 20 20" fill="none">
            <path d="M5 3H15C15.5523 3 16 3.44772 16 4V16C16 16.5523 15.5523 17 15 17H5C4.44772 17 4 16.5523 4 16V4C4 3.44772 4.44772 3 5 3Z" stroke="currentColor" stroke-width="1.5"/>
            <path d="M7 8H13M7 11H13M7 14H11" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span class="nav-label">Reports</span>
        </router-link>
      </nav>
      <div class="sidebar-footer">
        <LanguageSwitcher />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>
    </aside>

    <div class="main">
      <FilterBar />
      <main class="main-content">
        <router-view />
      </main>
    </div>

    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />

    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted, computed } from 'vue'
import { api } from './api'
import { useAuth } from './composables/useAuth'
import { useI18n } from './composables/useI18n'
import FilterBar from './components/FilterBar.vue'
import ProfileMenu from './components/ProfileMenu.vue'
import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
import TasksModal from './components/TasksModal.vue'
import LanguageSwitcher from './components/LanguageSwitcher.vue'

export default {
  name: 'App',
  components: {
    FilterBar,
    ProfileMenu,
    ProfileDetailsModal,
    TasksModal,
    LanguageSwitcher
  },
  setup() {
    const { currentUser } = useAuth()
    const { t } = useI18n()
    const showProfileDetails = ref(false)
    const showTasks = ref(false)
    const apiTasks = ref([])

    // ── Sidebar collapse ──────────────────────────────────────
    const userCollapsed = ref(localStorage.getItem('sidebarCollapsed') === 'true')
    const isNarrow = ref(false)
    let mq = null

    const handleMqChange = (e) => {
      isNarrow.value = e.matches
    }

    const collapsed = computed(() => isNarrow.value || userCollapsed.value)

    const toggleSidebar = () => {
      userCollapsed.value = !userCollapsed.value
      localStorage.setItem('sidebarCollapsed', String(userCollapsed.value))
    }
    // ─────────────────────────────────────────────────────────

    // Merge mock tasks from currentUser with API tasks
    const tasks = computed(() => {
      return [...currentUser.value.tasks, ...apiTasks.value]
    })

    const loadTasks = async () => {
      try {
        apiTasks.value = await api.getTasks()
      } catch (err) {
        console.error('Failed to load tasks:', err)
      }
    }

    const addTask = async (taskData) => {
      try {
        const newTask = await api.createTask(taskData)
        // Add new task to the beginning of the array
        apiTasks.value.unshift(newTask)
      } catch (err) {
        console.error('Failed to add task:', err)
      }
    }

    const deleteTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const isMockTask = currentUser.value.tasks.some(t => t.id === taskId)

        if (isMockTask) {
          // Remove from mock tasks
          const index = currentUser.value.tasks.findIndex(t => t.id === taskId)
          if (index !== -1) {
            currentUser.value.tasks.splice(index, 1)
          }
        } else {
          // Remove from API tasks
          await api.deleteTask(taskId)
          apiTasks.value = apiTasks.value.filter(t => t.id !== taskId)
        }
      } catch (err) {
        console.error('Failed to delete task:', err)
      }
    }

    const toggleTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const mockTask = currentUser.value.tasks.find(t => t.id === taskId)

        if (mockTask) {
          // Toggle mock task status
          mockTask.status = mockTask.status === 'pending' ? 'completed' : 'pending'
        } else {
          // Toggle API task
          const updatedTask = await api.toggleTask(taskId)
          const index = apiTasks.value.findIndex(t => t.id === taskId)
          if (index !== -1) {
            apiTasks.value[index] = updatedTask
          }
        }
      } catch (err) {
        console.error('Failed to toggle task:', err)
      }
    }

    onMounted(() => {
      loadTasks()
      mq = window.matchMedia('(max-width: 1024px)')
      isNarrow.value = mq.matches
      mq.addEventListener('change', handleMqChange)
    })

    onUnmounted(() => {
      if (mq) mq.removeEventListener('change', handleMqChange)
    })

    return {
      t,
      showProfileDetails,
      showTasks,
      tasks,
      addTask,
      deleteTask,
      toggleTask,
      collapsed,
      isNarrow,
      toggleSidebar
    }
  }
}
</script>

<style>
:root {
  --color-ink: #0f172a; --color-text: #1e293b; --color-text-muted: #64748b;
  --surface: #ffffff; --surface-subtle: #f8fafc; --surface-hover: #f1f5f9;
  --color-border: #e2e8f0; --color-border-strong: #cbd5e1;
  --primary: #2563eb; --primary-strong: #1e40af; --primary-soft: #eff6ff;
  --success: #059669; --success-soft: #d1fae5;
  --warning: #ea580c; --warning-soft: #fed7aa;
  --danger: #dc2626; --danger-soft: #fecaca;
  --space-1: 0.25rem; --space-2: 0.5rem; --space-3: 0.75rem; --space-4: 1rem;
  --space-5: 1.25rem; --space-6: 1.5rem; --space-8: 2rem;
  --radius-sm: 6px; --radius: 10px; --radius-lg: 12px;
  --shadow-sm: 0 1px 2px rgba(15,23,42,.04), 0 1px 3px rgba(15,23,42,.06);
  --sidebar-w: 248px; --content-max: 1600px; --content-gutter: var(--space-8);
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: #f8fafc;
  color: #1e293b;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ── Shell ────────────────────────────────────────────────── */

.app {
  display: grid;
  grid-template-columns: var(--sidebar-w) 1fr;
  min-height: 100vh;
}

.sidebar {
  position: sticky;
  top: 0;
  align-self: start;
  height: 100vh;
  display: flex;
  flex-direction: column;
  background: var(--surface);
  border-right: 1px solid var(--color-border);
  padding: var(--space-5) var(--space-3);
  overflow-y: auto;
  transition: padding 0.2s ease;
}

.logo {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  padding: var(--space-3) var(--space-4);
  margin-bottom: var(--space-4);
}

.brand-text {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
}

.logo h1 {
  font-size: 1.125rem;
  font-weight: 700;
  color: var(--color-ink);
  letter-spacing: -0.025em;
  line-height: 1.3;
}

.subtitle {
  font-size: 0.75rem;
  color: var(--color-text-muted);
  font-weight: 400;
}

.nav-tabs {
  display: flex;
  flex-direction: column;
  gap: var(--space-1);
}

.nav-tabs a {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-3) var(--space-4);
  border-radius: var(--radius-sm);
  color: var(--color-text);
  text-decoration: none;
  font-weight: 500;
  font-size: 0.938rem;
  border-left: 3px solid transparent;
  line-height: 1.3;
  transition: all 0.15s ease;
}

.nav-tabs a:hover {
  background: var(--surface-hover);
  color: var(--color-ink);
}

.nav-tabs a.active {
  background: var(--primary-soft);
  color: var(--primary-strong);
  border-left-color: var(--primary);
  font-weight: 600;
}

.sidebar-footer {
  margin-top: auto;
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}

.main {
  min-width: 0;
  display: flex;
  flex-direction: column;
}

.main-content {
  flex: 1;
  max-width: var(--content-max);
  width: 100%;
  margin: 0 auto;
  padding: var(--space-6) var(--content-gutter);
}

/* ── Sidebar toggle button ────────────────────────────────── */

.sidebar-toggle {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  flex-shrink: 0;
  border: none;
  border-radius: var(--radius-sm);
  background: transparent;
  color: var(--color-text-muted);
  cursor: pointer;
  transition: background 0.15s ease, color 0.15s ease;
}

.sidebar-toggle:hover {
  background: var(--surface-hover);
  color: var(--color-ink);
}

.sidebar-toggle svg {
  transition: transform 0.2s ease;
}

/* ── Nav icons ────────────────────────────────────────────── */

.nav-icon {
  width: 20px;
  height: 20px;
  flex: none;
  color: inherit;
}

/* ── Sidebar collapsed state ──────────────────────────────── */

.app.sidebar-collapsed {
  --sidebar-w: 64px;
}

.app.sidebar-collapsed .sidebar {
  padding: var(--space-5) var(--space-2);
  overflow: visible;
}

.app.sidebar-collapsed .sidebar-toggle svg {
  transform: rotate(180deg);
}

.app.sidebar-collapsed .logo {
  justify-content: center;
  padding: var(--space-3) 0;
}

.app.sidebar-collapsed .brand-text {
  display: none;
}

.app.sidebar-collapsed .nav-tabs a {
  justify-content: center;
  padding-left: 0;
  padding-right: 0;
  gap: 0;
}

.app.sidebar-collapsed .nav-label {
  display: none;
}

/* Footer controls — hide text + chevron, center the icon */

.app.sidebar-collapsed .language-button .language-label,
.app.sidebar-collapsed .language-button .chevron,
.app.sidebar-collapsed .profile-button .profile-name,
.app.sidebar-collapsed .profile-button .chevron {
  display: none;
}

.app.sidebar-collapsed .sidebar-footer .language-button,
.app.sidebar-collapsed .sidebar-footer .profile-button {
  justify-content: center;
  padding: var(--space-2);
  width: 100%;
}

/* Ensure collapsed-mode dropdowns can overflow the narrow rail */
.app.sidebar-collapsed .language-switcher .dropdown-menu,
.app.sidebar-collapsed .profile-menu .dropdown-menu {
  min-width: 200px;
}

/* ── Shared page-level styles ─────────────────────────────── */

.page-header {
  margin-bottom: 1.5rem;
}

.page-header h2 {
  font-size: 1.875rem;
  font-weight: 700;
  color: #0f172a;
  margin-bottom: 0.375rem;
  letter-spacing: -0.025em;
}

.page-header p {
  color: #64748b;
  font-size: 0.938rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.25rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: white;
  padding: 1.25rem;
  border-radius: 10px;
  border: 1px solid #e2e8f0;
  transition: all 0.2s ease;
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
}

.stat-label {
  color: #64748b;
  font-size: 0.875rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.625rem;
}

.stat-value {
  font-size: 2.25rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.stat-card.warning .stat-value {
  color: #ea580c;
}

.stat-card.success .stat-value {
  color: #059669;
}

.stat-card.danger .stat-value {
  color: #dc2626;
}

.stat-card.info .stat-value {
  color: #2563eb;
}

.card {
  background: white;
  border-radius: 10px;
  padding: 1.25rem;
  border: 1px solid #e2e8f0;
  margin-bottom: 1.25rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid #e2e8f0;
}

.card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: #f8fafc;
  border-top: 1px solid #e2e8f0;
  border-bottom: 1px solid #e2e8f0;
}

th {
  text-align: left;
  padding: 0.5rem 0.75rem;
  font-weight: 600;
  color: #475569;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: 0.5rem 0.75rem;
  border-top: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.875rem;
}

tbody tr {
  transition: background-color 0.15s ease;
}

tbody tr:hover {
  background: #f8fafc;
}

.badge {
  display: inline-block;
  padding: 0.313rem 0.75rem;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.025em;
}

.badge.success {
  background: #d1fae5;
  color: #065f46;
}

.badge.warning {
  background: #fed7aa;
  color: #92400e;
}

.badge.danger {
  background: #fecaca;
  color: #991b1b;
}

.badge.info {
  background: #dbeafe;
  color: #1e40af;
}

.badge.increasing {
  background: #d1fae5;
  color: #065f46;
}

.badge.decreasing {
  background: #fecaca;
  color: #991b1b;
}

.badge.stable {
  background: #e0e7ff;
  color: #3730a3;
}

.badge.high {
  background: #fecaca;
  color: #991b1b;
}

.badge.medium {
  background: #fed7aa;
  color: #92400e;
}

.badge.low {
  background: #dbeafe;
  color: #1e40af;
}

.loading {
  text-align: center;
  padding: 3rem;
  color: #64748b;
  font-size: 0.938rem;
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  font-size: 0.938rem;
}
</style>
