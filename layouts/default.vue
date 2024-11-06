<template>
   <main class="overflow-y-auto min-h-screen relative bg-[url('/takar-hisab-bg.jpg')] bg-cover bg-center bg-no-repeat bg-fixed">
    <div class="bg-white/60 dark:bg-gray-900/90 w-full min-h-screen">
        <nav class="fixed bg-white/30 dark:bg-gray-900/80 top-0 left-0 right-0 w-full backdrop-blur-md  border-b border-slate-200 dark:border-gray-700 z-20">
            <TopBar />
        </nav>
        <aside class="fixed top-[55px] left-0 bottom-0 p-3 shadow-lg border-r border-slate-200 dark:border-gray-700 bg-white/30 dark:bg-gray-900/80">
            <SideBar />
        </aside>
        <div class="ml-56 p-5 pt-16">
            <slot />
        </div>
    </div>
   </main>
</template>