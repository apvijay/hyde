---
layout: page
title: Rolling Shutter Demo
---

Under Construction

<!---------------------------------------------------------------------------->
<canvas id="canvas256" width="256" height="256"></canvas><br >
<canvas id="canvas_exp" width="512" height="128"></canvas>
<canvas id="canvas_mot" width="512" height="128"></canvas>
<div>
  <input id="warpingbtn" value="Warping" type="button">
  <input id="reloadbtn" value="Reload" type="button">
<!--   <input id="drawbtn" value="Draw" type="button"> -->
  <br >
  Middle row reference? <input id="warp_cen_check" type="checkbox" value="check">
  <br >
  Camera Parameters
  <br >
  Live update? <input id="live_check" type="checkbox" value="check">
  <br >
  Focal Length
  <input id="flen_box" type="range" min="1" max="1000" step="1" value="1">:
  <output id="flen_box_disp">1</output>
  <br >
  Row exposure time
  <input id="t_e_box" type="range" min="1" max="100" step="1" value="1">:
  <output id="t_e_box_disp">1</output>
  <br >
  Top to bottom row delay time
  <input id="T_r_box" type="range" min="0" max="100" step="1" value="30">:
  <output id="T_r_box_disp">30</output>
  <br >
  Poses per row exposure
  <input id="pose_per_t_e_box" type="range" min="1" max="20" step="1" value="5">:
  <output id="pose_per_t_e_box_disp">5</output>
  <br >
  Scene distance
  <input id="dist_box" type="range" min="1" max="100" step="1" value="1">:
  <output id="dist_box_disp">1</output>
  <br >
  Motion simulation end time
  <input id="T_e_box" type="range" min="1" max="1000" step="1" value="100">:
  <output id="T_e_box_disp">100</output>
  <br >
  Translations are in
  <input id="trans_unit_mm_box" type="radio" name="trans_unit_check" value="mm">mm, 
  <input id="trans_unit_pix_box" type="radio" name="trans_unit_check" value="pixel" checked="true">pixel
  <br >
  When pixel option selected, the translation values are applied directly on the image plane and, focal length and distance to scene are ignored by them.
  <br >
  p0 p1 p2 p3
  <br >
  tx
  <input id="tx_p0_box" type="text" size = "4" value="0">
  <input id="tx_p1_box" type="text" size = "4" value="-100">
  <input id="tx_p2_box" type="text" size = "4" value="-20">
  <input id="tx_p3_box" type="text" size = "4" value="140">
  <br >
  ty
  <input id="ty_p0_box" type="text" size = "4" value="0">
  <input id="ty_p1_box" type="text" size = "4" value="0">
  <input id="ty_p2_box" type="text" size = "4" value="0">
  <input id="ty_p3_box" type="text" size = "4" value="0">
  <br >
  tz
  <input id="tz_p0_box" type="text" size = "4" value="0">
  <input id="tz_p1_box" type="text" size = "4" value="0">
  <input id="tz_p2_box" type="text" size = "4" value="0">
  <input id="tz_p3_box" type="text" size = "4" value="0">
  <br >
  rx
  <input id="rx_p0_box" type="text" size = "4" value="0">
  <input id="rx_p1_box" type="text" size = "4" value="0">
  <input id="rx_p2_box" type="text" size = "4" value="0">
  <input id="rx_p3_box" type="text" size = "4" value="0">
  <br >
  ry
  <input id="ry_p0_box" type="text" size = "4" value="0">
  <input id="ry_p1_box" type="text" size = "4" value="0">
  <input id="ry_p2_box" type="text" size = "4" value="0">
  <input id="ry_p3_box" type="text" size = "4" value="0">
  <br >
  rz
  <input id="rz_p0_box" type="text" size = "4" value="0">
  <input id="rz_p1_box" type="text" size = "4" value="0">
  <input id="rz_p2_box" type="text" size = "4" value="0">
  <input id="rz_p3_box" type="text" size = "4" value="0">
</div>

<script type="text/javascript">



var img = new Image();
var canvas = document.getElementById('canvas256');
var ctx = canvas.getContext('2d');

img.src = '../img/orig256.png';
// ctx.drawImage(img, 0, 0);
// var imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
// var img_pix = imageData.data;
// var img_pix_orig = img_pix.slice();

// for (var i = 0; i < 12; i+=1) {
//   console.log(img_pix[i]);
// }
// ctx.putImageData(imageData, 0,0);

  ctx.drawImage(img, 0,0);
  imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
  img_pix = imageData.data;
  img_pix_orig = img_pix.slice();
  ctx.putImageData(imageData, 0,0);

var reload = function() {
  ctx.drawImage(img, 0,0);
  imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
  img_pix = imageData.data;
  img_pix_orig = img_pix.slice();
  ctx.putImageData(imageData, 0,0);
}

function getPixelIndex(x, y) {
  return y*imageData.width + x;
}


var apply_homo = function(x,y,H) {
  xd = Number(Math.round((H[0]*x + H[3]*y + H[6]) / (H[2]*x + H[5]*y + H[8])));
  yd = Number(Math.round((H[1]*x + H[4]*y + H[7]) / (H[2]*x + H[5]*y + H[8])));
  return [xd, yd] //yd*imageData.width + xd
}

var get_homo = function(flen,dist,tx,ty,tz,rx,ry,rz) {
  cx = Math.cos(rx);  cy = Math.cos(ry);  cz = Math.cos(rz);
  sx = Math.sin(rx);  sy = Math.sin(ry);  sz = Math.sin(rz);
  H = [0,0,0,0,0,0,0,0,0];
  H[0] = cy * cz;
  H[3] = -cy * sz;
  if(document.getElementById('trans_unit_pix_box').checked == true) {
    H[6] = -sy * flen + tx;
    H[7] = (-sx * cy ) * flen + ty;
    H[8] = cx * cy + tz;
  }else if(document.getElementById('trans_unit_mm_box').checked == true) {
    H[6] = (-sy + tx/dist) * flen;
    H[7] = (-sx * cy + ty/dist) * flen;
    H[8] = cx * cy + tz/dist;
  }
  
  H[1] = cx * sz - sx * sy * cz;
  H[4] = cx * cz + sx * sy * sz;
  
  H[2] = (sx * sz + cx * sy * sz) / flen;
  H[5] = (sx * cz - cx * sy * sz) / flen;
  
  return H;

}
document.getElementById("flen_box").oninput = function() {
  val = document.getElementById("flen_box").value;
  document.getElementById('flen_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("T_r_box").oninput = function() {
  val = document.getElementById("T_r_box").value;
  document.getElementById('T_r_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("t_e_box").oninput = function() {
  val = document.getElementById("t_e_box").value;
  document.getElementById('t_e_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("pose_per_t_e_box").oninput = function() {
  val = document.getElementById("pose_per_t_e_box").value;
  document.getElementById('pose_per_t_e_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("dist_box").oninput = function() {
  val = document.getElementById("dist_box").value;
  document.getElementById('dist_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};
document.getElementById("T_e_box").oninput = function() {
  val = document.getElementById("T_e_box").value;
  document.getElementById('T_e_box_disp').innerHTML = val;
  if(document.getElementById('live_check').checked == true) {
    rsmb_warp();
  }
};


var rsmb_warp = function() {
  // exp_draw();

  //if(document.getElementById('trans_unit_pix_box').checked == true)

  var flen = Number(document.getElementById('flen_box').value);
  var t_e = Number(document.getElementById('t_e_box').value);
  var T_r = Number(document.getElementById('T_r_box').value);
  var pose_per_te = Number(document.getElementById('pose_per_t_e_box').value);
  var T_e = Number(document.getElementById('T_e_box').value);

  var dist = Number(document.getElementById('dist_box').value);

  var tx_p0 = Number(document.getElementById('tx_p0_box').value);
  var tx_p1 = Number(document.getElementById('tx_p1_box').value);
  var tx_p2 = Number(document.getElementById('tx_p2_box').value);
  var tx_p3 = Number(document.getElementById('tx_p3_box').value);
  var ty_p0 = Number(document.getElementById('ty_p0_box').value);
  var ty_p1 = Number(document.getElementById('ty_p1_box').value);
  var ty_p2 = Number(document.getElementById('ty_p2_box').value);
  var ty_p3 = Number(document.getElementById('ty_p3_box').value);
  var tz_p0 = Number(document.getElementById('tz_p0_box').value);
  var tz_p1 = Number(document.getElementById('tz_p1_box').value);
  var tz_p2 = Number(document.getElementById('tz_p2_box').value);
  var tz_p3 = Number(document.getElementById('tz_p3_box').value);
  var rx_p0 = Number(document.getElementById('rx_p0_box').value);
  var rx_p1 = Number(document.getElementById('rx_p1_box').value);
  var rx_p2 = Number(document.getElementById('rx_p2_box').value);
  var rx_p3 = Number(document.getElementById('rx_p3_box').value);
  var ry_p0 = Number(document.getElementById('ry_p0_box').value);
  var ry_p1 = Number(document.getElementById('ry_p1_box').value);
  var ry_p2 = Number(document.getElementById('ry_p2_box').value);
  var ry_p3 = Number(document.getElementById('ry_p3_box').value);
  var rz_p0 = Number(document.getElementById('rz_p0_box').value);
  var rz_p1 = Number(document.getElementById('rz_p1_box').value);
  var rz_p2 = Number(document.getElementById('rz_p2_box').value);
  var rz_p3 = Number(document.getElementById('rz_p3_box').value);

//  document.getElementById('t_e_box_disp').innerHTML = t_e;

//  var t_e = 10; // single row exposure in ms
//  var T_r = 30; // total line delay in ms
  var t_r = T_r / (imageData.height-1); // single line delay
//  var pose_per_te = 10;
  // var T_e = 100; //t_e + T_r; // total image exposure

        yidx_cen =   (imageData.height/2 * t_r) / T_e;
  if (document.getElementById('warp_cen_check').checked == true) {
    tx_cen = tx_p0 + (tx_p1 * yidx_cen) + (tx_p2 * Math.pow(yidx_cen,2)) + (tx_p3 * Math.pow(yidx_cen,3));
    ty_cen = ty_p0 + ty_p1 * yidx_cen + ty_p2 * Math.pow(yidx_cen,2) + ty_p3 * Math.pow(yidx_cen,3);
    tz_cen = tz_p0 + tz_p1 * yidx_cen + tz_p2 * Math.pow(yidx_cen,2) + tz_p3 * Math.pow(yidx_cen,3);
    rx_cen = (rx_p0 + rx_p1 * yidx_cen + rx_p2 * Math.pow(yidx_cen,2) + rx_p3 * Math.pow(yidx_cen,3))*Math.PI/180;
    ry_cen = (ry_p0 + ry_p1 * yidx_cen + ry_p2 * Math.pow(yidx_cen,2) + ry_p3 * Math.pow(yidx_cen,3))*Math.PI/180;
    rz_cen = (rz_p0 + rz_p1 * yidx_cen + rz_p2 * Math.pow(yidx_cen,2) + rz_p3 * Math.pow(yidx_cen,3))*Math.PI/180;
  }
  
  for (y = 0; y < imageData.height; y += 1) {
    for (n = 0; n < pose_per_te; n += 1) {

      yidx =   (y * t_r + n * t_e/pose_per_te) / T_e; // (1/pose_per_te) * // (y + n/pose_per_te);

      tx = tx_p0 + (tx_p1 * yidx) + (tx_p2 * Math.pow(yidx,2)) + (tx_p3 * Math.pow(yidx,3));
      ty = ty_p0 + ty_p1 * yidx + ty_p2 * Math.pow(yidx,2) + ty_p3 * Math.pow(yidx,3);
      tz = tz_p0 + tz_p1 * yidx + tz_p2 * Math.pow(yidx,2) + tz_p3 * Math.pow(yidx,3);
      rx = (rx_p0 + rx_p1 * yidx + rx_p2 * Math.pow(yidx,2) + rx_p3 * Math.pow(yidx,3))*Math.PI/180;
      ry = (ry_p0 + ry_p1 * yidx + ry_p2 * Math.pow(yidx,2) + ry_p3 * Math.pow(yidx,3))*Math.PI/180;
      rz = (rz_p0 + rz_p1 * yidx + rz_p2 * Math.pow(yidx,2) + rz_p3 * Math.pow(yidx,3))*Math.PI/180;

      if (document.getElementById('warp_cen_check').checked == true) {
        tx = tx - tx_cen;
        ty = ty - ty_cen;
        tz = tz - tz_cen;
        rx = rx - rx_cen;
        ry = ry - ry_cen;
        rz = rz - rz_cen;
      }
      for (x = 0; x < imageData.width; x += 1) {
        H = get_homo(flen,dist,-tx,-ty,-tz,-rx,-ry,-rz);
        // if(x==0 && y==0 & n==0) console.log(H,tx,flen,dist);
        tmp = apply_homo(x-imageData.width/2,y-imageData.height/2, H);
        xd = tmp[0]+imageData.width/2;
        yd = tmp[1]+imageData.height/2;
        
        i = getPixelIndex(x,y);
        j = getPixelIndex(xd,yd);

        if (n == 0) { 
          if (xd >=0 && yd >= 0 && xd < imageData.width && yd < imageData.height) {
            img_pix[4*i] = img_pix_orig[4*j] / pose_per_te; 
            img_pix[4*i+1] = img_pix_orig[4*j+1] / pose_per_te; 
            img_pix[4*i+2] = img_pix_orig[4*j+2] / pose_per_te; 
          }
          else {
            img_pix[4*i] = 120 / pose_per_te;
            img_pix[4*i+1] = 0 / pose_per_te;
            img_pix[4*i+2] = 0 / pose_per_te;
          }
        } 
        else {
          if (xd >=0 && yd >= 0 && xd < imageData.width && yd < imageData.height) {
            img_pix[4*i] += img_pix_orig[4*j] / pose_per_te; 
            img_pix[4*i+1] += img_pix_orig[4*j+1] / pose_per_te;
            img_pix[4*i+2] += img_pix_orig[4*j+2] / pose_per_te; 
          }
          else {
            img_pix[4*i] += 120 / pose_per_te;
            img_pix[4*i+1] += 0 / pose_per_te;
            img_pix[4*i+2] += 0 / pose_per_te;
          }
        } // end if n == 0
       
      } // end for x
    } // end for n
  } // end for y
  
  ctx.putImageData(imageData, 0,0);
  
  FAC = 512 / T_e;
  canvas_exp = document.getElementById('canvas_exp');
  if (canvas_exp.getContext) {
    ctx_exp = canvas_exp.getContext('2d');
    ctx_exp.clearRect(0, 0, 512, 128);

    ctx_exp.fillStyle = 'rgb(240,240,180)';
    ctx_exp.fillRect(0, 0, 512, 128);
    // ctx_exp.clearRect(45, 45, 60, 60);
    // ctx_exp.strokeRect(50, 50, 50, 50);
    
    ctx_exp.translate(0,0);
    ctx_exp.beginPath();
    ctx_exp.lineTo(T_r * FAC, imageData.height);
    ctx_exp.lineTo((T_r+t_e) * FAC, imageData.height);
    ctx_exp.lineTo(t_e * FAC, 0);
    ctx_exp.lineTo(0, 0);
    ctx_exp.closePath();
    ctx_exp.fillStyle = "rgb(150, 120, 80)";
    ctx_exp.fill();
    ctx_exp.lineWidth = 1.0;
    //ctx_exp.lineCap = "round";
    //ctx_exp.lineJoin = "round";
    // ctx_exp.stroke();  
  }

  canvas_mot = document.getElementById('canvas_mot');
  if (canvas_mot.getContext) {
    ctx_mot = canvas_mot.getContext('2d');
    ctx_mot.clearRect(0, 0, 512, 128);

    ctx_mot.translate(0,0);

    ctx_mot.fillStyle = 'rgb(240,240,230)';
    ctx_mot.fillRect(0, 0, 512, 128);
    ctx_mot.fillStyle = 'rgb(120,120,0)';
    
    for (t = 0; t < T_e; t+=1) {
      tx_plot = tx_p0 + (tx_p1 * t/T_e) + (tx_p2 * Math.pow(t/T_e,2)) + (tx_p3 * Math.pow(t/T_e,3));
      tx_plot = 64 - tx_plot; // 64->0, -64->128
      ctx_mot.beginPath();
      // ctx_mot.lineTo(t * FAC, tx_plot);
      ctx_mot.arc(t * FAC, tx_plot, 0.5, 0, Math.PI * 2, true);   
      ctx_mot.closePath();
      ctx_mot.stroke();
    }
    
  }
}


var warpingbtn = document.getElementById('warpingbtn');
warpingbtn.addEventListener('click', rsmb_warp);
var reloadbtn = document.getElementById('reloadbtn');
reloadbtn.addEventListener('click', reload);
// var drawbtn = document.getElementById('drawbtn');
// drawbtn.addEventListener('click', exp_draw);
</script>


<!---------------------------------------------------------------------------->