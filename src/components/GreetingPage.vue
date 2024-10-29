<script setup>
  import Logotype from './LogotypeSVG.vue';
</script>

<template>
  <div class="greeting" id="greeting">
    <div class="greeting__logotype">
      <Logotype />
    </div>
    <div class="greeting__view">
      <h1 class="greeting__title">Атмосферный<br>квартал</h1>
      <div class="greeting__button button">Узнать подробности</div>
      <div class="greeting__annotation">
        <span>Застройщик ООО “СЗ СУ1-ЗАТОН”. Проектная декларация размещена на сайте <a href="https://xn--80az8a.xn--d1aqf.xn--p1ai/" target="_blank">наш.дом.рф</a></span>
        <a href="https://veter-ufa.ru/upload/conf_policy_processing.pdf" target="_blank">Политика конфиденциальности</a>
      </div>
    </div>
  </div>
  <div class="form-overlay form-overlay--greeting">
    <div id="greetingForm">
      <div class="close">
        <div class="close__line"></div>
        <div class="close__line"></div>
      </div>
      <div class="weather">
        <span>Ожидаются осадки<br> в виде плюсов</span>
        <img src="./icons/formWeatherIcon.svg" class="weather__icon" loading="lazy"/>
      </div>
      <form class="form greeting__form">
        <span class="modal-title">Узнайте<br>подробности</span>
        <div class="form__group">
          <input id="name" name="name" type="text" placeholder="Имя" class="form__input" required/>
        </div>
        <div class="form__group">
          <input id="phone" name="phone" pattern=".{0}|.{18,}" type="tel" placeholder="Телефон" ref="phoneInput" class="form__input phone" required/>
        </div>
        <div class="form__buttons">
          <button type="submit" class="button button--transparent">Узнать подробности</button>
          <div class="form__rules">
            <input id="rules" name="rules" type="checkbox" checked class="form__checkbox" required/>
            <label for="rules"><span>Нажимая кнопку, Вы соглашаетесь<br> <a href="https://veter-ufa.ru/upload/conf_policy_processing.pdf" target="_blank"> на обработку персональных данных.</a></span></label>
          </div>
        </div>
      </form>
    </div>
    <div id="successForm">
      <span class="modal-title">Данные<br>успешно<br>отправлены!</span>
      <p>Мы перезвоним Вам в<br> в ближайшее время</p>
      <div class="button">Закрыть</div>
    </div>
  </div>
</template>

<script>
  import $ from "jquery";
  import IMask from 'imask';

  $(document).ready(function () {

    // ОТКРЫТИЕ МОДАЛКИ
    $('.greeting__button').on('click', function () {
        $('#greetingForm').css("display", "block").fadeIn(400);		
        $('.form-overlay').css("display", "flex").fadeIn(400);
        return false;
    });

    $("#greetingForm .close").on('click', function () {
      $(this).parents('.form-overlay').fadeOut(400);
      $('#greetingForm').css("display", "none").fadeOut(400);

      
      if ($('.form-overlay').hasClass("sended")) {
        $(this).parents('.form-overlay').removeClass("sended");
        $(this).parents('.form-overlay').fadeOut(400);
        $('#successForm').fadeOut(400);
      }
    });

    $(document).keydown(function(e) {
      if (e.keyCode === 27) {
        e.stopPropagation();
        $('#greetingForm').css("display", "none").fadeOut(400);		
        $('.form-overlay').fadeOut(400);

        if ($('.form-overlay').hasClass("sended")) {
          console.log("hello2")
          $('.form-overlay').removeClass("sended");
          $('.form-overlay').fadeOut(400);
          $('#successForm').fadeOut(400);
        }
      }
    });

    $('.form-overlay').click(function(e) {
      if ($(e.target).closest('#greetingForm').length == 0) {
        $('#greetingForm').css("display", "none").fadeOut(400);		
        $(this).fadeOut(400);			

        if ($(this).hasClass("sended")) {
          $(this).removeClass("sended");
          $(this).fadeOut(400);
          $('#successForm').fadeOut(400);
        }		
      }
    });
  });

  // ПИСЬМО УСПЕШНО ОТПРАВЛЕНО
  $(document).ready(function() {
    $('.greeting__form').on('submit', function(event) {
      event.preventDefault(); // предотвращает стандартную отправку формы

      // Имитируем успешный ответ
      const response = { success: true };

      // Проверка на успех и отображение модального окна
      if (response.success) {
        $('#greetingForm').fadeOut(400, function() {		
          $('#successForm').fadeIn(400);
          $('.form-overlay').addClass("sended");
        });

        // Очистка полей формы
        $('.greeting__form')[0].reset();
        
       
        setTimeout(function(){
          if ($('.form-overlay').hasClass("sended")) {
            console.log("hello");
            $('.form-overlay').fadeOut(400);
            $('#successForm').fadeOut(400);
          }
        }, 5000);
      }
    });
  });

  export default {
    mounted() {
      //PHONE MASK
      const phoneInput = this.$refs.phoneInput;
      IMask(phoneInput, {
        mask: '+{7} (000) 000-00-00',
      });
    },
  };
</script>