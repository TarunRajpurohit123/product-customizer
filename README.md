# product-customizer

<!-- customizer.liquid -->

<style>
  .customize-btn {
    padding: 14px 32px;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white; border: none; border-radius: 8px; cursor: pointer;
    font-size: 16px; font-weight: 600;
    box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
    transition: all 0.3s ease;
  }
  .customize-btn:hover { transform: translateY(-2px); }

  .customizer-modal {
    position: fixed; left: 0; top: 0; width: 100%; height: 100%;
    background: rgba(0,0,0,0.7); backdrop-filter: blur(4px);
    display: none; justify-content: center; align-items: center;
    z-index: 99999; padding:20px;
  }
  .customizer-modal-content {
    background:white; width:100%; max-width:1000px; padding:30px;
    border-radius:16px; position:relative; max-height:90vh;
    overflow-y:auto; animation: slideUp .4s ease;
  }

  .close-modal {
    position:absolute; right:20px; top:20px; background:#f3f4f6; border:none;
    width:36px; height:36px; border-radius:50%; font-size:24px; cursor:pointer;
  }

  .customizer-container { display:flex; gap:30px; margin-top:20px; }
  .customizer-left { width:59%; }
  .customizer-right { width:39%; }

  .mockup-box { background:#f5f7fa; border-radius:12px; text-align:center; }
  .canvas-wrap { width:100%; border-radius: 8px; overflow:hidden; background:white; }

  #preview-canvas { width:100% !important; height:auto !important; display:block; }

  .upload-box{
    display:flex; flex-direction:column; gap:10px; padding:30px;
    border:2px dashed #d1d5db; border-radius:8px; background:#f9fafb;
    cursor:pointer;
  }
  .upload-input{display:none;}
</style>


<button id="open-customizer" class="customize-btn">Customize Product</button>

<div id="customizer-modal" class="customizer-modal">
  <div class="customizer-modal-content">
    <button id="close-customizer" class="close-modal">×</button>

    <div class="customizer-container">
      
      <div class="customizer-left">
        <div class="mockup-box">
          <div class="canvas-wrap">
            <canvas id="preview-canvas" width="902" height="817"></canvas>
          </div>
        </div>
      </div>

      <div class="customizer-right">
        <label class="upload-box">
          <input type="file" class="upload-input" id="image-upload" accept="image/*">
          <span>Upload 1 Image</span>
        </label>

        <button id="finish-now">finish</button>
      </div>

    </div>
  </div>
</div>


<script src="https://cdnjs.cloudflare.com/ajax/libs/fabric.js/5.3.0/fabric.min.js"></script>

<script>
document.addEventListener("DOMContentLoaded", function () {

  const BASE_W = 554;
  const BASE_H = 500;

  const modal = document.getElementById('customizer-modal');
  const openBtn = document.getElementById('open-customizer');
  const closeBtn = document.getElementById('close-customizer');
  const imageUpload = document.getElementById('image-upload');
  const finishBtn = document.getElementById("finish-now");

  const canvas = new fabric.Canvas('preview-canvas', {
    width: BASE_W,
    height: BASE_H,
    preserveObjectStacking: true
  });

  const baseImageURL =
    "https://cdn.shopify.com/s/files/1/0687/7793/5093/files/Cushion_12.jpg?v=1763026338";

  /* Load Base Pillow Image */
  function loadBaseImage() {
    fabric.Image.fromURL(baseImageURL, function(img) {
      img.set({
        left: 0, top: 0,
        originX: "left", originY: "top",
        selectable: false, evented: false
      });

      img.scaleToWidth(BASE_W);
      img.scaleToHeight(BASE_H);

      canvas.setBackgroundImage(img, canvas.renderAll.bind(canvas));
    }, { crossOrigin: "anonymous" });
  }


  /* ===== Pillow Polygon JSON Path ===== */
  const pillowJson = [
  {
    "command": "M",
    "values": [
      94.3338,
      0.5
    ]
  },
  {
    "command": "H",
    "values": [
      70.8338
    ]
  },
  {
    "command": "V",
    "values": [
      60
    ]
  },
  {
    "command": "C",
    "values": [
      57.2338,
      80,
      70.5004,
      113.667,
      78.8337,
      128
    ]
  },
  {
    "command": "L",
    "values": [
      95.8337,
      463.5
    ]
  },
  {
    "command": "C",
    "values": [
      106.634,
      474.7,
      106,
      484.833,
      104.334,
      488.5
    ]
  },
  {
    "command": "C",
    "values": [
      79.9337,
      512.1,
      82.5004,
      566,
      86.8337,
      590
    ]
  },
  {
    "command": "C",
    "values": [
      65.2337,
      616.4,
      64.5004,
      662.667,
      66.8338,
      682.5
    ]
  },
  {
    "command": "C",
    "values": [
      66.8338,
      692.1,
      77.8337,
      725.5,
      83.3337,
      741
    ]
  },
  {
    "command": "C",
    "values": [
      90.9337,
      765,
      80.1671,
      773.333,
      73.8337,
      774.5
    ]
  },
  {
    "command": "C",
    "values": [
      67.4337,
      778.1,
      66.5004,
      787,
      66.8338,
      791
    ]
  },
  {
    "command": "C",
    "values": [
      -18.3662,
      828.2,
      -3.66624,
      857.5,
      14.3338,
      867.5
    ]
  },
  {
    "command": "C",
    "values": [
      47.8338,
      879.5,
      195.834,
      879.667,
      264.834,
      877
    ]
  },
  {
    "command": "L",
    "values": [
      296.334,
      863.5
    ]
  },
  {
    "command": "L",
    "values": [
      320.834,
      874
    ]
  },
  {
    "command": "C",
    "values": [
      372.034,
      884.4,
      720.5,
      878.333,
      888.334,
      874
    ]
  },
  {
    "command": "C",
    "values": [
      888.334,
      866,
      911.334,
      862,
      922.834,
      861
    ]
  },
  {
    "command": "C",
    "values": [
      958.034,
      857,
      967.167,
      852.667,
      967.334,
      851
    ]
  },
  {
    "command": "C",
    "values": [
      979.334,
      831,
      942.667,
      806.667,
      922.834,
      797
    ]
  },
  {
    "command": "L",
    "values": [
      917.334,
      631
    ]
  },
  {
    "command": "C",
    "values": [
      915.667,
      636.167,
      912.834,
      642.6,
      914.834,
      627
    ]
  },
  {
    "command": "C",
    "values": [
      916.834,
      611.4,
      910,
      556.5,
      906.334,
      531
    ]
  },
  {
    "command": "C",
    "values": [
      906.334,
      488.667,
      904.934,
      401.5,
      899.334,
      391.5
    ]
  },
  {
    "command": "C",
    "values": [
      893.734,
      381.5,
      897,
      363.667,
      899.334,
      356
    ]
  },
  {
    "command": "V",
    "values": [
      311.5
    ]
  },
  {
    "command": "C",
    "values": [
      901.734,
      294.7,
      905,
      273.167,
      906.334,
      264.5
    ]
  },
  {
    "command": "C",
    "values": [
      901.534,
      248.5,
      908.334,
      201.833,
      912.334,
      180.5
    ]
  },
  {
    "command": "C",
    "values": [
      924.334,
      135.7,
      934.334,
      75.1667,
      937.834,
      50.5
    ]
  },
  {
    "command": "C",
    "values": [
      941.834,
      18.5,
      930.834,
      13.5,
      924.834,
      15
    ]
  },
  {
    "command": "C",
    "values": [
      892.034,
      12.6,
      783.167,
      53,
      732.834,
      73.5
    ]
  },
  {
    "command": "C",
    "values": [
      712.034,
      89.9,
      651.167,
      110.667,
      623.334,
      119
    ]
  },
  {
    "command": "C",
    "values": [
      586.934,
      123.4,
      561.834,
      143.833,
      553.834,
      153.5
    ]
  },
  {
    "command": "C",
    "values": [
      541.434,
      165.9,
      513.334,
      183.333,
      500.834,
      190.5
    ]
  },
  {
    "command": "C",
    "values": [
      495.234,
      196.1,
      490.167,
      191.5,
      488.334,
      188.5
    ]
  },
  {
    "command": "C",
    "values": [
      487.934,
      184.9,
      472.5,
      170.667,
      464.834,
      164
    ]
  },
  {
    "command": "C",
    "values": [
      457.234,
      156.8,
      401,
      127.667,
      373.834,
      114
    ]
  },
  {
    "command": "C",
    "values": [
      341.834,
      91.2,
      174.167,
      28.8333,
      94.3338,
      0.5
    ]
  },
  {
    "command": "Z",
    "values": []
  }
];

  function jsonToSVGPath(json) {
    return json.map(cmd => cmd.command + " " + cmd.values.join(" ")).join(" ");
  }

  const pillowSVG = jsonToSVGPath(pillowJson);
  let clipShape;


  /* Load Polygon Clip Path */
  function loadClipPath() {
    clipShape = new fabric.Path(pillowSVG, {
      left: 0,
      top: 0,
      stroke: "yellow",
      strokeWidth: 2,
      fill: "transparent",
      selectable: true,
      hasControls: true,
      hasBorders: true,
      objectCaching: false
    });

    canvas.add(clipShape);

    /* Keep clipping working if polygon moves */
    canvas.on("object:modified", () => {
      canvas.getObjects().forEach(obj => {
        if (obj.type === "image" && obj !== clipShape) {
          clipShape.absolutePositioned = true;
          obj.clipPath = clipShape;
        }
      });
      canvas.renderAll();
    });
  }


  /* ===== Upload Design ===== */
  imageUpload.addEventListener("change", (e) => {
    const file = e.target.files[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = (ev) => {
      fabric.Image.fromURL(ev.target.result, function(img) {

        img.set({
          left: BASE_W / 2,
          top: BASE_H / 2,
          originX: "center",
          originY: "center",
          cornerStyle: "circle",
          cornerSize: 12,

          /* ⭐ Increased transparency */
          opacity: 0.78
        });

        clipShape.absolutePositioned = true;
        img.clipPath = clipShape;

        canvas.add(img);
        canvas.setActiveObject(img);


        /* ⭐ STRONGER SHADOW OVERLAY ⭐ */
        const shadowURL =
          "https://cdn.shopify.com/s/files/1/0687/7793/5093/files/pillow_shadow.png";

        fabric.Image.fromURL(shadowURL, function(shadowImg) {
          shadowImg.set({
            left: img.left,
            top: img.top,
            originX: "center",
            originY: "center",
            selectable: false,
            evented: false,

            /* ⭐ Stronger shadow */
            opacity: 0.62,
            objectCaching: false
          });

          shadowImg.clipPath = clipShape;
          shadowImg.moveTo(canvas.getObjects().length);

          canvas.add(shadowImg);
          canvas.renderAll();
        }, { crossOrigin: "anonymous" });
      });
    };

    reader.readAsDataURL(file);
  });


  /* Hide Polygon on Finish */
  finishBtn.addEventListener("click", () => {
    if (clipShape) {
      clipShape.set({
        stroke: "rgba(0,0,0,0)",
        strokeWidth: 0,
        fill: "rgba(0,0,0,0)",
        visible: false,
        selectable: false,
        hasControls: false,
        hasBorders: false,
        evented: false
      });

      canvas.discardActiveObject();
      canvas.renderAll();
    }
  });


  /* Open / Close Modal */
  openBtn.onclick = () => {
    modal.style.display = "flex";
    loadBaseImage();
    loadClipPath();
  };

  closeBtn.onclick = () => {
    modal.style.display = "none";
  };

});
</script>
