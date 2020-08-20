# browser-notifivation-and-PWA

~~~
<style>
.add-button {
  position: absolute;
  top: 1px;
  left: 1px;
display:none;
}
</style>
<button class="add-button">Add to home screen</button>

<script>

  if ('serviceWorker' in navigator) {

window.addEventListener("load", function () {
        navigator.serviceWorker.register('<?php echo base_url(); ?>service-worker.js');
    });


    console.log("Will the service worker register?");
    navigator.serviceWorker.register('<?php echo base_url(); ?>service-worker.js')
      .then(function(reg){
        console.log("Yes, it did.");
      }).catch(function(err) {
        console.log("No it didn't. This happened: ", err)
      });
  }

  <?php //if($this->session->userdata('role') == '5') : ?>

    function fireNotification() {
      var notification = new Notification("Appointment", {
         icon:'https://telemedicine-demo.fmv.cc/favicon-32x32.png',
         body: 'A new appointment is just added, Would like to check.',
      });

      notification.onclick = function () {
         window.open( '<?php echo base_url('front/Appointment#list')?>' );      
      };
    }

    /* Enable Browser Notification */
    if (Notification.permission !== "granted") {
      Notification.requestPermission();
    }
  <?php //endif; ?>


//Call the checkBrowserNottifictionAjaxCall() function every 1000 millisecond
var $ijk = 0;
if( $ijk == 0 ) {
  checkBrowserNottifictionAjaxCall();

  $ijk = 1;
}

setInterval("checkBrowserNottifictionAjaxCall()", 60 * 1000);

function checkBrowserNottifictionAjaxCall(){
  $.ajax({
    "url": "<?php echo base_url('front/CronBrowerNotification')?>",
    dataType: 'JSON',
    type: 'GET',
    success: function(data){
        console.log( 'aa', data );
        if( data.data == 'true' ) {
          fireNotification();
        }
    }
  });
}
</script>
~~~
