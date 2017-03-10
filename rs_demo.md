---
layout: page
title: Rolling Shutter Demo
---

Under Construction

<!---------------------------------------------------------------------------->
<canvas id="canvas256" width="256" height="256"></canvas>
<div>
  <input id="warpingbtn" value="Warping" type="button">
  <input id="reloadbtn" value="Reload" type="button">
  <br >
  Middle row reference? <input id="warp_cen_check" type="checkbox" value="check">
  <br >
  Camera Parameters
  <br >
  Focal Length
  <input id="flen_box" type="range" min="1" max="1000" step="1" value="1">:
  <output id="flen_box_disp">1</output>
  <br >
  Row exposure time
  <input id="t_e_box" type="range" min="1" max="100" step="1" value="1">:
  <output id="t_e_box_disp">1</output>
  Live update? <input id="t_e_live_check" type="checkbox" value="check">
  <br >
  Top to bottom row delay time
  <input id="T_r_box" type="range" min="1" max="100" step="1" value="30">:
  <output id="T_r_box_disp">30</output>
  <br >
  Poses per row exposure
  <input id="pose_per_te_box" type="text" size = "4" value="5">.
  <br >
  Scene distance
  <input id="dist_box" type="text" size = "10" value="1">
  <br >
  p0 p1 p2 p3
  <br >
  tx
  <input id="tx_p0_box" type="text" size = "10" value="0">
  <input id="tx_p1_box" type="text" size = "10" value="0">
  <input id="tx_p2_box" type="text" size = "10" value="0">
  <input id="tx_p3_box" type="text" size = "10" value="0">
  <br >
  ty
  <input id="ty_p0_box" type="text" size = "10" value="0">
  <input id="ty_p1_box" type="text" size = "10" value="0">
  <input id="ty_p2_box" type="text" size = "10" value="0">
  <input id="ty_p3_box" type="text" size = "10" value="0">
  <br >
  tz
  <input id="tz_p0_box" type="text" size = "10" value="0">
  <input id="tz_p1_box" type="text" size = "10" value="0">
  <input id="tz_p2_box" type="text" size = "10" value="0">
  <input id="tz_p3_box" type="text" size = "10" value="0">
  <br >
  rx
  <input id="rx_p0_box" type="text" size = "10" value="0">
  <input id="rx_p1_box" type="text" size = "10" value="0">
  <input id="rx_p2_box" type="text" size = "10" value="0">
  <input id="rx_p3_box" type="text" size = "10" value="0">
  <br >
  ry
  <input id="ry_p0_box" type="text" size = "10" value="0">
  <input id="ry_p1_box" type="text" size = "10" value="0">
  <input id="ry_p2_box" type="text" size = "10" value="0">
  <input id="ry_p3_box" type="text" size = "10" value="0">
  <br >
  rz
  <input id="rz_p0_box" type="text" size = "10" value="0">
  <input id="rz_p1_box" type="text" size = "10" value="0">
  <input id="rz_p2_box" type="text" size = "10" value="0">
  <input id="rz_p3_box" type="text" size = "10" value="0">
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
  H[6] = (-sy + tx/dist) * flen;
  H[1] = cx * sz - sx * sy * cz;
  H[4] = cx * cz + sx * sy * sz;
  H[7] = (-sx * cy + ty/dist) * flen;
  H[2] = (sx * sz + cx * sy * sz) / flen;
  H[5] = (sx * cz - cx * sy * sz) / flen;
  H[8] = cx * cy + tz/dist;
  return H;

}
document.getElementById("flen_box").oninput = function() {
  val = document.getElementById("flen_box").value;
  document.getElementById('flen_box_disp').innerHTML = val;
};
document.getElementById("T_r_box").oninput = function() {
  val = document.getElementById("T_r_box").value;
  document.getElementById('T_r_box_disp').innerHTML = val;
};
document.getElementById("t_e_box").oninput = function() {
  val = document.getElementById("t_e_box").value;
  document.getElementById('t_e_box_disp').innerHTML = val;
  if(document.getElementById('t_e_live_check').checked == true) rsmb_warp();
};

var rsmb_warp = function() {
  
  var flen = Number(document.getElementById('flen_box').value);
  var t_e = Number(document.getElementById('t_e_box').value);
  var T_r = Number(document.getElementById('T_r_box').value);
  var pose_per_te = Number(document.getElementById('pose_per_te_box').value);

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
  var T_e = 100; //t_e + T_r; // total image exposure

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
  
}


var warpingbtn = document.getElementById('warpingbtn');
warpingbtn.addEventListener('click', rsmb_warp);
var reloadbtn = document.getElementById('reloadbtn');
reloadbtn.addEventListener('click', reload);
</script>


<!---------------------------------------------------------------------------->