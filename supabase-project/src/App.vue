<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'

import { createClient } from '@supabase/supabase-js'

const supabaseUrl = 'https://pfezbxbambvjwwoluxkm.supabase.co'
const supabaseKey = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InBmZXpieGJhbWJ2and3b2x1eGttIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjIwNjYwNzMsImV4cCI6MjA3NzY0MjA3M30.93nRluY6_I6IsV6LEjzi8F-5yE9SEFYbHwYI9L7RLU0'
const supabase = createClient(supabaseUrl, supabaseKey)

const { data: signInData, error: signInError }= await supabase.auth.signInWithPassword({
  email: 'scliu.leo@outlook.com',
  password: 'Supabase123!'
})

const { data: data, error: error } = await supabase
  .from('user_info')
  .insert([
    {
      first_name: 'Leo',
      last_name: 'thinker',
      age: 28,
      user_id: signInData?.user?.id
    }
  ])
  .select()

console.log('数据:', data)
console.log('错误:', error)
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
