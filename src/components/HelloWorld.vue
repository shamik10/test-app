<template>
  <div class="test-app" ref="copyHTML">
    <div class="header">
      <img id="retrunBack" @click="undo()" src="../assets/back_btn.svg" alt="">
      <img @click="redo()" src="../assets/forward_btn.svg" alt="">
      <img @click="makeHeading()" src="../assets/heding.svg" alt="">
      <img @click="makeParagraph()" src="../assets/Paragraph.svg" alt="">
      <img @click="insertImage()" src="../assets/add_photo.svg" alt="">
      <h6 class="copy-text-html" @click="copyHtmlFunc()">Скопировать текст в html</h6>
    </div>
    <div>
      <p contenteditable="true" ref="editableDiv">
        {{ texts.firstText }}
      </p>
    </div>
    <div>
      <h2>Смотрите какие обезьянки</h2>
      <img src="../assets/monkey.png" alt="">
    </div>
    <div >
      <p contenteditable="true" ref="editableDiv">
        Таким образом консультация с широким активом представляет собой интересный эксперимент проверки позиций, занимаемых участниками в отношении поставленных задач. С другой стороны постоянное информационно-пропагандистское обеспечение нашей деятельности представляет собой инйцу шо шщйоц ущойц ущошцщйуо йцщуо йщцоу щйоу шщйцош ущйтересный эксперимент проверки форм развития. Идейные соображения высшего порядка, а также укрепление и развитие структуры влечет за собой процесс внедрения и модернизации соответствующий условий активизации. Задача организации, в особенности же реализация намеченных плановых заданий играет важную роль в формировании дальнейших направлений развития. Повседневная практика показывает, что постоянное информационно-пропагандистское обеспечение нашей деятельности играет важную роль в формировании существенных финансовых и административных <br> <br>Товарищи! новая модель организационной деятельности требуют от нас анализа направлений прогрессивного развития. Задача организации, в особенности же постоянный количественный рост и сфера нашей активности требуют от нас анализа позиций, занимаемых участниками в отношении поставленных задач. Задача организации, в особенности же реализация намеченных плановых заданий требуют от нас анализа системы обучения кадров, соответствует насущным потребностям.
      </p>
    </div>
  </div>
</template>

<script setup>
  import { onMounted, reactive, ref } from 'vue';
  import axios from 'axios';

  const text = ref('');
  const editableDiv = ref(null);
  const copyHTML = ref(null);
  const imageUrl = ref('');
  const editor = ref(null);

  const texts = reactive({
    firstText: '',
    secondText: ''
  })

  const getText = async () => {
    const {data} = await axios.get('https://ca7a66c41effefaf.mokky.dev/userText');
    text.value = data;
    for (const elem of data) {
      texts.firstText = elem.firstText;
      texts.secondText = elem.secondText;
    }
  }

 
  const undo = () => {
    if (editableDiv.value) {
      console.log(editableDiv.value);
      editableDiv.value.focus(); 
      document.execCommand('undo', false, null);
    }
  };

  const redo = () => {
    if (editableDiv.value) {
      editableDiv.value.focus();
      document.execCommand('redo')
    }
  }

  const makeHeading = () => {
    console.log('saf');
  if (!editableDiv.value) return;

  const selection = window.getSelection();
  if (selection.rangeCount > 0) {
    const range = selection.getRangeAt(0);
    const selectedText = range.toString().trim(); 

    if (selectedText) {
      const h2 = document.createElement('h2');
      h2.textContent = selectedText;
      range.deleteContents(); 
      range.insertNode(h2); 
    }
  }
  };

  const makeParagraph = () => {
    console.log('saf');
  if (!editableDiv.value) return;

  const selection = window.getSelection();
  if (selection.rangeCount > 0) {
    const range = selection.getRangeAt(0);
    const selectedText = range.toString().trim(); 
    if (selectedText) {
      const p = document.createElement('p');
      p.textContent = selectedText;
      range.deleteContents(); 
      range.insertNode(p); 
    }
  }
  };

  const copyHtmlFunc = () => {
  if (copyHTML.value) {
    const html = copyHTML.value.innerHTML;
    navigator.clipboard.writeText(html)
      .then(() => {
        alert('HTML скопирован');
      })
      .catch(err => {
        console.error('Ошибка при копировании:', err);
        alert('Не удалось скопировать HTML. посмотрите в консол браузера.');
      });
  }
};


function insertImageAtCursor(imageUrl) {
  const selection = window.getSelection();

  if (selection && selection.rangeCount > 0) {
    const range = selection.getRangeAt(0);

    const img = document.createElement('img');
    img.src = imageUrl;
    img.alt = "ошибка"; 

    range.insertNode(img);
    range.setStartAfter(img);
    range.collapse(true);
    selection.removeAllRanges();
    selection.addRange(range);

  }
}

const insertImage = () => {
  imageUrl.value =  prompt('введите ссылку:');
  insertImageAtCursor(imageUrl.value);
};
  

onMounted(
  () => { 
    getText();
  }
)
</script>

<style scoped>
  .test-app {
    display: flex;
    flex-direction: column;
    justify-content: start;
    color: white;
  }


  .header {
    display: flex;
    width: 450px;
    gap: 8px;
    cursor: pointer;
  }

  .copy-text-html {
    padding-left: 10px;
    font-size: 15px;
    font-weight: 400;
    color: #639EFF
  }
  
</style>
