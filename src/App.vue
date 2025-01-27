

<template>
<div class="row h-100">
  <div class="col-3">
    <div class="col-9 bg-warning h-100 right-window">
      <div class="inner-window">
        <div class="first-column">
        </div>
        <div class="main-column">
          <ul class="list">
              <li class="list-item">Лист1</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</div>
  
</template>

<script>

</script>


<style>
    body { font-family: Arial, Helvetica, sans-serif; }
    .fixed-box {
        position: fixed; width: 100%; }
    
    .sticky-header {
        position: fixed; }
    
    .inner-window {
        display: flex; flex-direction: row; }
    
    .first-column {
        overflow-y: auto; height: 400px; width: 10%; background-color: #ECEFFA; padding: 20px; margin-right: 24px; }
    
    .main-column {
        overflow-y: auto; height: 400px; width: 90%; }

    .list {
        }

    .list-item {
        font-size: 18px; margin: 0 16px 16px 0; height: 244px; width: 252px; border-radius: 12px; display: inline-block; text-align: center; }

    h1 { 
        font-size: 30px; line-height: 42px; }

</style>
