<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <q-header >
        <q-toolbar style="background-color: #f0f0f0;">
          <q-space></q-space>
          <q-item
           clickable
            to="/"
            tag="a"
            target="_blank"
            class="text-h7">
            <q-item-section>
              <q-item-label style="color: teal;">صفحه اصلی</q-item-label>
            </q-item-section>
          </q-item>
          <div>
            <a href="/">
              <img
              class="text-logo"
              src="src/assets/goldis-logo-horizontal.png"
              width="140"
              >

            </a>
        </div>
        </q-toolbar>
      </q-header>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

