<script lang="ts" setup>   
import { SearchIcon } from 'lucide-vue-next';
import { Input } from '../components/ui/input';
import LoginDialog  from './LoginDialog.vue';
import RegisterDialog  from './RegisterDialog.vue';
</script>
<template>
  <header className="bg-background/80 supports-[backdrop-filter]:bg-background/60 sticky top-0 z-30 border-b backdrop-blur">
    <div className="mx-auto flex h-14 max-w-7xl items-center gap-2 px-4">
 
        <div className="mx-auto hidden max-w-xl flex-1 items-center md:flex">
          <div className="relative w-full">
            <SearchIcon className="pointer-events-none absolute top-1/2 left-3 size-4 -translate-y-1/2 text-purple-500" />
            <Input
              placeholder="🔍 Search..."
              className="w-full border-purple-200 pl-9 focus:border-purple-400"
              aria-label="Search"
            />
          </div>
        </div>
        <div className="flex items-center justify-between">
            <LoginDialog /> 
            <RegisterDialog />
        </div>
      </div>
    
  </header>
</template>