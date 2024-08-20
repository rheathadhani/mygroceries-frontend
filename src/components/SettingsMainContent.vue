<template>
    <div class="container">
        <div class="sidebar">
            <h2>Manage <br> your acessibility</h2>
        <ul>
            <li @click="currentComponent='ColourModeSettings'">Colour Contrast</li>
            <li @click="currentComponent='SpeechSettings'">Speech</li>
            <li @click="currentComponent='FontSettings'">Text Visibility</li>
        </ul>
        </div>
        <div class="content">
            <component :is="currentComponent"></component>
        </div>
    </div>
</template>


<script>
import SpeechSettings from './SpeechSettings.vue';
import ColourModeSettings from './ColourModeSettings.vue';
import FontSettings from './FontSettings.vue';

export default {
    name:'SettingsMainContent',
    components: {
        SpeechSettings,
        ColourModeSettings,
        FontSettings
    },
    data(){
        return{
            currentComponent: 'ColourModeSettings'
        }
    }
}
</script>

<style scoped>
.container {
  display: flex;
  height: 100vh;
}

h2 {
  color: #000;
  margin-left: 20px;
}

.sidebar {
    display: flex;
    flex-direction: column;
    border: 1px;
    color: white; /* White text */
    padding: 10px;
    width: 20%;
    margin-top: 25px; 
}

.profile {
  margin-bottom: 20px;
}

.profile-pic {
  width: 100px;
  height: 100px;
  border-radius: 50%;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background-color: white;
  color: black;
  padding: 20px 20px;
  border-bottom: 1px solid #ccc; /* Lighter border for a subtle look */
  cursor: pointer;
  transition: background-color 0.3s;
} 

li:hover {
  background-color: #f7f7f7;
}

.content {
  flex: 1;
  background-color: #fff;
  color: #000;
  padding: 20px;
  margin-left: 20px;
  /*background-color: #f7f7f7;*/
}
</style>