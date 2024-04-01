<script>
	let src = "https://cdn.glitch.global/1a805310-f4e7-42c5-8f7a-7deafb272818/default.png?v=1692155518638", time, today

	function compressImage(file, size = 1080) {
		return new Promise((resolve, reject) => {
			const reader = new FileReader();
			reader.readAsDataURL(file);
			reader.onload = function(event) {
				const img = new Image();
				img.src = event.target.result;
				img.onload = function() {
					const canvas = document.createElement('canvas');
					const ctx = canvas.getContext('2d');

					// Asegura que la imagen no se amplíe si es más pequeña que size x size
					let scaleFactor = 1;
					if (img.width > size || img.height > size) {
						scaleFactor = Math.min(size / img.width, size / img.height);
					}
					const width = Math.round(img.width * scaleFactor);
					const height = Math.round(img.height * scaleFactor);

					canvas.width = width;
					canvas.height = height;

					ctx.drawImage(img, 0, 0, width, height);
					canvas.toBlob(
						(blob) => { if (blob) resolve(blob); else reject("Error al convertir a WebP");},
						'image/webp', 0.7
					);
				};
				img.onerror = function() { reject("Error al cargar la imagen");};
			};
			reader.onerror = function() { reject("Error al leer el archivo"); };
		});
	}

	async function handleImage(event) {
		const file = event.target.files[0];
		if (file) {
		const compressedFile = await compressImage(file, 600);
		const compressedSrc = URL.createObjectURL(compressedFile);
		src = compressedSrc;
		}
	}

	function currentTime() {
		let date = new Date(); 
		let hh = date.getHours();
		let mm = date.getMinutes();
		let ss = date.getSeconds();
		
		let dd = String(date.getDate()).padStart(2, '0');
		let months = String(date.getMonth() + 1).padStart(2, '0'); //January is 0!
		let yyyy = date.getFullYear();
		
		today = dd + '-' + months + '-' + yyyy;

		if(hh == 0){
			hh = 24;
		}
		if(hh > 24){
			hh = hh - 24;
		}

		hh = (hh < 10) ? "0" + hh : hh;
		mm = (mm < 10) ? "0" + mm : mm;
		ss = (ss < 10) ? "0" + ss : ss;
			
		time = hh + ":" + mm + ":" + ss;

		let t = setTimeout(function(){ currentTime() }, 1000);
	}

	currentTime();
</script>

<div class="body">
	<header>
		<svg width="139" height="25" viewBox="0 0 169 42" fill="none" xmlns="http://www.w3.org/2000/svg">
			<path d="M127.062 10.4385C124.286 11.1771 122.175 12.9313 120.767 15.6548C119.751 17.6167 119.751 23.0869 120.728 25.3719C121.588 27.3337 123.387 29.2033 125.42 30.1496C127.453 31.119 130.698 31.2806 132.809 30.4958C134.1 30.0111 136.641 28.3724 136.641 28.0262C136.641 27.9338 137.658 28.4416 138.909 29.1571C143.835 31.973 149.7 31.5114 152.124 28.1185C152.984 26.8952 153.297 23.5485 152.671 22.0482C152.006 20.3864 149.895 19.0477 146.259 17.986C144.109 17.3628 143.288 16.9935 143.131 16.555C142.349 14.7777 145.751 14.5469 148.683 16.2088L149.934 16.9473L150.677 15.9549C151.068 15.4009 151.694 14.4546 152.085 13.8545L152.788 12.7466L151.303 11.8465C149.269 10.6232 147.745 10.2308 144.851 10.2308C142.506 10.2308 142.349 10.277 140.746 11.2233C138.674 12.4466 137.697 14.2007 137.697 16.6935C137.697 20.0402 139.143 21.4943 143.796 22.833C145.282 23.2484 146.767 23.7331 147.041 23.8716C147.706 24.1947 147.745 25.5103 147.119 25.8796C145.868 26.6182 142.584 25.9027 140.903 24.564C140.394 24.1717 139.886 23.8485 139.73 23.8485C139.573 23.8485 139.065 24.4255 138.596 25.118L137.736 26.3874L135.781 24.8641L133.787 23.3407L132.692 24.4025C130.229 26.7336 127.492 26.1797 125.811 23.0407C124.13 19.9248 126.476 15.3086 129.682 15.3086C130.464 15.3086 132.106 16.1626 133.122 17.1089C133.709 17.6629 133.709 17.6629 135.585 16.1395C136.602 15.2855 137.423 14.4777 137.423 14.3161C137.423 13.716 135.82 12.0773 134.491 11.2925C133.2 10.5309 131.128 10 129.408 10C128.978 10 127.922 10.2077 127.062 10.4385Z" fill="#137BEE"/>
			<path d="M102.115 17.5783C102.267 25.1027 102.688 27.4006 104.369 29.0227C105.897 30.4645 108.151 31.1178 111.13 30.9826C113.919 30.8249 115.715 30.059 117.166 28.3243C118.656 26.5671 119 24.4044 119 17.2854V11H116.326H113.652L113.613 17.353C113.613 24.2241 113.422 24.945 111.703 25.711C110.328 26.2967 109.258 26.0489 108.227 24.9C107.387 23.9988 107.348 23.6384 107.234 17.4656L107.081 11H104.56H102L102.115 17.5783Z" fill="#137BEE"/>
			<path d="M153 21V31H155.796H158.592V28.2414V25.4828L160.31 25.4598C163.785 25.4368 166.421 24.4023 167.939 22.4943C169.217 20.8851 169.377 16.3793 168.219 14.3793C167.3 12.8161 165.622 11.6667 163.665 11.2529C163.026 11.1149 160.39 11 157.753 11H153V21ZM162.467 16.2644C163.545 16.8621 163.945 18.2874 163.385 19.4828C162.906 20.5402 162.067 20.8851 160.03 20.8851H158.592V18.3563V15.8276H160.11C161.069 15.8276 161.947 15.9885 162.467 16.2644Z" fill="#137BEE"/>
			<path d="M61 13.4529C61 25.015 61.0241 27.0624 61.1807 27.9777C61.8912 32.0124 63.9869 35.6376 66.9978 38.0825C69.792 40.3226 72.7427 41.5029 76.2474 41.7799C84.7985 42.4423 92.3139 36.5409 93.8193 27.9777C93.9759 27.0624 94 25.015 94 13.4529V-8.39266e-06H77.5H61V13.4529ZM76.0066 2.3124C75.9584 2.46897 75.838 2.91459 75.7175 3.31204C75.4285 4.3478 75.4285 7.46714 75.7296 8.53904C76.2836 10.5744 76.946 11.6102 79.3066 14.2117C82.6788 17.9091 83.4858 19.7639 83.3653 23.5456C83.3051 25.5328 83.0522 26.6529 82.3416 28.2186C81.2336 30.6514 79.1139 32.7591 76.7051 33.8551C75.862 34.2405 74.2964 34.6861 73.8026 34.6861C73.5376 34.6861 73.5135 34.65 73.5496 34.1321L73.5858 33.5781L74.6818 33.3011C78.2588 32.3978 80.8964 29.712 81.9562 25.8942C82.3536 24.485 82.3657 22.0883 81.9803 20.631C81.2215 17.7044 79.2223 15.1511 76.6931 13.8744C75.296 13.1639 73.0318 12.6942 71.719 12.8507L71.1168 12.923V14.0792V15.2354L72.4175 15.2474C73.4653 15.2595 73.8628 15.3197 74.5854 15.5726C75.8259 16.0062 76.7051 16.5602 77.6204 17.4755C80.1015 19.9806 80.7036 23.654 79.1259 26.8336C78.054 28.9894 75.9343 30.7117 73.923 31.0369L73.5255 31.1091V27.1106V23.1241H75.2117H76.8978V21.9197V20.7153H75.2117H73.5255V18.6197V16.5241L72.8631 16.4398C72.5018 16.4036 71.9599 16.4036 71.6588 16.4398L71.1168 16.5241V18.6197V20.7153H69.4307H67.7445V21.9197V23.1241H69.4307H71.1168V30.7117C71.1168 34.8788 71.0807 38.2993 71.0325 38.2993C70.888 38.2993 69.6354 37.5164 68.8526 36.9624C67.865 36.2398 66.227 34.5296 65.4803 33.4456C64.1193 31.4704 63.3124 29.2182 63.0474 26.7252C62.9752 25.9785 62.927 21.0405 62.927 13.7781V2.04744H69.5029H76.0788L76.0066 2.3124ZM92.0489 3.39634L92.0128 4.75729L85.0033 4.79342C78.3672 4.81751 77.9818 4.80547 77.9818 4.61277C77.9818 4.27554 78.2828 3.23977 78.5599 2.6135L78.8128 2.04744H85.4489H92.085L92.0489 3.39634ZM92.073 6.56386V7.9489H85.0996H78.1263L78.054 7.67189C78.0058 7.52736 77.9456 6.90109 77.8974 6.28685L77.8252 5.17882H84.9551H92.073V6.56386ZM92.0489 9.71933L92.085 11.0803H85.9547H79.8245L79.3427 10.3817C78.7887 9.61094 78.3431 8.75583 78.3431 8.51496C78.3431 8.38247 79.5354 8.35839 85.1839 8.35839L92.0128 8.37043L92.0489 9.71933ZM92.073 12.8869V14.2117H87.3277H82.5704L82.2934 13.8624C82.1369 13.6697 81.619 13.0796 81.1252 12.5376L80.246 11.562H86.1595H92.073V12.8869ZM92.0489 16.0423L92.0128 17.4033L88.3876 17.4394L84.7624 17.4635L84.3891 16.777C84.1843 16.3916 83.823 15.8135 83.5942 15.4763C83.3533 15.1511 83.1606 14.8379 83.1606 14.7898C83.1606 14.7296 85.1719 14.6934 87.6168 14.6934H92.085L92.0489 16.0423ZM92.0489 19.2339L92.085 20.5949H88.8934H85.7139L85.5212 19.7759C85.4248 19.3303 85.2562 18.7281 85.1478 18.439C85.0515 18.15 84.9672 17.8971 84.9672 17.873C84.9672 17.8369 86.5569 17.8369 88.496 17.8489L92.0128 17.885L92.0489 19.2339ZM92.073 22.4014V23.7263H89.0018H85.9307V23.3288C85.9307 23.1 85.8945 22.5098 85.8584 22.004L85.7741 21.0766H88.9296H92.073V22.4014ZM92.073 24.8945C92.073 25.2799 92.0369 25.8821 92.0007 26.2193L91.9164 26.8577H88.6766C85.9186 26.8577 85.4369 26.8336 85.485 26.689C85.5212 26.5927 85.6175 26.0628 85.6898 25.5087C85.7741 24.9668 85.8584 24.4369 85.8825 24.3525C85.9186 24.2442 86.6412 24.208 89.0018 24.208H92.073V24.8945ZM91.8201 27.6646C91.8201 27.8573 91.6996 28.4595 91.5551 29.0255L91.2901 30.0493L87.7493 30.0854C84.931 30.1095 84.2325 30.0854 84.2807 29.965C84.5336 29.4109 85.196 27.7489 85.2442 27.5682C85.3044 27.3514 85.4971 27.3394 88.5682 27.3394H91.8321L91.8201 27.6646ZM90.9891 30.6514C90.9891 30.7478 90.712 31.362 90.3628 32.0365L89.7365 33.2409H85.8343H81.9321L82.462 32.6025C82.7511 32.2653 83.2208 31.639 83.5099 31.2175L84.0157 30.4708H87.4964C90.4832 30.4708 90.9891 30.4949 90.9891 30.6514ZM89.1945 33.9394C89.1343 34.0598 88.6164 34.65 88.0383 35.2642L86.9905 36.3723H82.2212H77.4398L78.4996 35.8062C79.0777 35.4931 79.969 34.9029 80.4869 34.4934L81.4142 33.7347H85.3646C89.1223 33.7226 89.3029 33.7347 89.1945 33.9394ZM85.8343 37.2033C85.2803 37.685 83.269 38.7087 82.173 39.0701C79.5956 39.9372 76.6449 40.0817 74.188 39.4675L73.5858 39.323L73.5496 38.3354L73.5135 37.3478L73.9712 37.2755C74.2241 37.2394 74.7299 37.131 75.0912 37.0347C75.5971 36.9142 77.0303 36.8781 80.9927 36.866H86.2318L85.8343 37.2033Z" fill="#137BEE"/>
			<path d="M13 10.5L2.5 21L13 31.5" stroke="#137BEE" stroke-width="3" stroke-linecap="round"/>
		</svg>            
	</header>
	
	<main>
		<h3 class="white">Carnet Virtual</h3>
		<article>
			<div class="img-container">
				<input type="file" name="img" id="img" on:change={handleImage} accept="image/x-png,image/jpeg,image/gif">
				<label for="img">
					<img id="blah" width="180" height="228" {src} alt="no-seo">
				</label>
				<div class="small">Regular</div>
			</div>
			<div class="texts">
				<div class="name big white" contenteditable>Haz click y escribe tu nombre</div>
				<div>Código académico:</div>
				<div contenteditable>Tu código</div>
				<div class="margin">Alumno Regular</div>
				<div class="small" contenteditable>Nombre de la carrera</div>
				<div class="small">Pregrado</div>
			</div>
			<div class="hour-container">
				<div id="hora">{time}</div>
				<div id="fecha" class="big">{today}</div>
			</div>
		</article>
	</main>
	
	<footer>
		<svg width="56" height="59" viewBox="0 0 56 59" fill="none" xmlns="http://www.w3.org/2000/svg">
			<rect width="56" height="59" rx="7" fill="#137BEE"/>
			<path d="M32.5 11.5H22.5V15H28.5V22.5H32.5V11.5Z" fill="white"/>
			<path d="M26.5 18.5H22.5V26.5H8.5V35.5H12V29.5H26.5V18.5Z" fill="white"/>
			<path d="M19.5 32.5H14.5V35.5H19.5V32.5Z" fill="white"/>
			<path d="M22.5 45.5V48.5H25.5V45.5H22.5Z" fill="white"/>
			<path fill-rule="evenodd" clip-rule="evenodd" d="M19.5 38.5H8.5V49.5H19.5V38.5ZM11.5 41.5V46.5H16.5V41.5H11.5Z" fill="white"/>
			<path d="M28.5 32.5H22.5V42H28.5V49.5H32.5V38.5H26.5V35.5H32.5V29.5H35.5V32.5H38.5V25.5H28.5V32.5Z" fill="white"/>
			<path d="M43.5 35.5H35.5V38.5H46.5V26.5H43.5V35.5Z" fill="white"/>
			<path d="M47 42H35V49H39V45.5H43V49H47V42Z" fill="white"/>
			<path fill-rule="evenodd" clip-rule="evenodd" d="M19.5 22.5V11.5H8.5V22.5H19.5ZM16 15H12V19H16V15Z" fill="white"/>
			<path fill-rule="evenodd" clip-rule="evenodd" d="M35.5 11.5V22.5H46.5V11.5H35.5ZM39 15H43V19H39V15Z" fill="white"/>
		</svg>
	</footer>
</div>

<style>
	.name {
		margin-bottom: 10px;
	}
	.margin {
		margin: 5px 0;
	}
	.body {
		box-sizing: border-box;
		display: flex;
		flex-direction: column;
		height: 100dvh;
		justify-content: space-between;
		padding: 16px 0;
	}
	article {
		display: flex;
		align-items: center;
		gap: 40px;
		padding: 16px 40px 0;
	}
	h3 {
		padding: 16px;
		text-align: center;
	}
	img {
		display: block;
		border-radius: 14px;
    	object-fit: cover;
	}
	input {
		display: none;
	}
	.img-container, .hour-container {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 4px;
		font-size: 14px;
	}
	.hour-container {
		padding: 0 16px;
	}
	footer {
		text-align: right;
    	padding-right: 16px;
	}
	footer svg {
		width: 32px;
		height: 32px;
	}
	#hora {
		font-size: 36px;
		line-height: 30px;
	}
	.big {
		font-size: 20px;
	}
	.small {
		font-size: 15px;
	}
	@media (orientation: portrait) {
		article {
			flex-direction: column;
		}
		.texts {
			text-align: center;
		}
	}
</style>