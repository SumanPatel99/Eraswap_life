 <script>
	let user = { loggedIn: false };

	function toggle() {
		user.loggedIn = !user.loggedIn;
	}
</script>



<style>
.tm-padding-section {
  padding-top: 80px;
  padding-bottom: 0px !important;
}


</style>
<!-- Footer Area -->
<div class="tm-footer bg-gradient" style="z-index: auto">
    <div class="container">
    <img src="/images/chatbot.png" alt="chatbot" class="open-button" onclick="openForm()" style="width:120px">

<div class="chat-popup" id="myForm">
  <form class="form-container">
     <iframe
             allow="microphone;"
             width="350"
             height="430"
             src="https://console.dialogflow.com/api-client/demo/embedded/11b7c637-920a-4280-8c9e-f948a97e3ac6">
    </iframe>
    <button type="button" class="btn cancel" onclick="closeForm()">Close</button>
  </form>
</div>
        <div class="tm-footer-toparea tm-padding-section" >
            <div class="row widgets tm-footer-widgets" style="padding-top: 0px !important; padding-bottom: 3px !important;">
                <div class="col-xl-3 col-lg- col-md-4 col-12">
                    <div class="single-widget widget-info" style="padding-right:0">
                        <a href="/" class="widget-info-logo">
                            <img src="/images/es_newlogo-white-min.png" width="180px" alt="">
                        </a>
                        <ul class="policy_list">
                            <li><a href="https://www.facebook.com/eraswap/" target="_blank"><i class="fa fa-facebook" style="font-size:22px"></i></a></li>
							<li><a href="https://www.linkedin.com/company/eraswap/" target="_blank"><i class="fa fa-linkedin" style="font-size:22px"></i></a></li>
							<li><a href="https://www.pinterest.com/eraswapt/" target="_blank"><i class="fa fa-pinterest" style="font-size:22px"></i></a></li>
							<li><a href="https://www.youtube.com/channel/UCGCP4f5DF1W6sbCjS6y3T1g" target="_blank"><i class="fa fa-youtube" style="font-size:22px"></i></a></li>
							<li><a href="https://www.instagram.com/eraswap/" target="_blank"><i class="fa fa-instagram" style="font-size:22px"></i></a></li>
							<li><a href="https://medium.com/@eraswap" target="_blank"><i class="fa fa-medium" aria-hidden="true" style="font-size:22px"></i></a></li>
                            <li><a href="https://coinmarketcap.com/currencies/era-swap/" target="_blank"><img src="/images/cmc.png" style="height: 22px; margin-bottom: 7px;" /></a></li>
                            <li style="margin-left:-4px"><a href="https://ethplorer.io/address/0xef1344bdf80bef3ff4428d8becec3eea4a2cf574" target="_blank"><img src="/images/ethplorer2.png" style="height: 22px; margin-bottom: 7px;" /></a></li>
                        </ul>
                        <!-- <p>Dolor enim sit et perferendis quia in. Sed perferendis aliquid quo vel
                            necessitatibus commodi similique et. Veniam ducimus sed rerum cumque ipsum eos
                            omnis eos blanditiis.</p> -->
                    </div>
                </div>

                <div class="col-xl-3 col-lg-4 col-md-4 col-12">
                    <div class="single-widget">
                        <!-- <h5 class="widget-title">Services</h5> -->
                        <ul>
                            <li><h4 class="text-white">Smart Contracts Address</h4></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="https://etherscan.io/address/0xef1344bdf80bef3ff4428d8becec3eea4a2cf574#code" target="_blank"><i class="zmdi zmdi-apple"></i><span>Era Swap Token (ES)</span></a></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="https://etherscan.io/address/0x20ee679d73559e4c4b5e3b3042b61be723828d6c#code" target="_blank"><i class="zmdi zmdi-apple"></i><span>Newly Released Token (NRT)</span></a></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="https://etherscan.io/address/0x42225682113E6Ed3616B36B4A72BbaE376041D7c#code" target="_blank"><i class="zmdi zmdi-apple"></i><span>BetDeEx DApp</span></a></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="https://etherscan.io/address/0x5630ee5f247bd6b61991fbb2f117bbeb45990876#code" target="_blank"><i class="zmdi zmdi-apple"></i><span>TimeAlly DApp</span></a></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="https://etherscan.io/address/0xbad9af4db5401b7d5e8177a18c1d69c35fc03fd3#code" target="_blank"><i class="zmdi zmdi-apple"></i><span>TSGAP DApp</span></a></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="https://etherscan.io/address/0x69e7960f6A1d6332a4be7e22916F627a3d95b1bc#code" target="_blank"><i class="zmdi zmdi-apple"></i><span>PET DApp</span></a></li>
                            <!-- <li><i class="fa fa-angle-double-right"></i> <a href="/access-my-wallet">Access my Wallet</a></li> -->
                        </ul>
                    </div>
                </div>
             <div class="col-xl-3 col-lg-4 col-md-4 col-12">
                    <div class="single-widget">
                        <!-- <h5 class="widget-title">Services</h5> -->
                        <ul>
                            <li><h4 class="text-white">Useful Links</h4></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="/pdf/EraSwapLife.pdf" target="_blank">User Guide</a></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="https://eraswaptoken.io/pdf/eraswap_whitepaper.pdf" target="_blank">Era Swap White Paper</a></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="https://eraswaptoken.io/pdf/eraswap_lightpaper.pdf" target="_blank"> Era Swap Light Paper</a></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="https://eraswap.life/pdf/esn-whitepaper.pdf" target="_blank"> ESN White Paper</a></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="https://eraswaptoken.io/era-swap-howey-test-letter-august7-2018.php">Howey Test</a></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="/create-new-wallet">Create Wallet</a></li>
                            <li><i class="fa fa-angle-double-right"></i> <a href="/access-my-wallet">Access my Wallet</a></li>
                        </ul>
                    </div>
                </div>
                <div class="col-xl-3 col-lg-4 col-md-4 col-12" on:click={toggle}>
                    <!-- <iframe src="https://www.youtube-nocookie.com/embed/WTydWvczn8g?rel=0&amp;showinfo=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> -->
                    <h4 class="text-white">Era Swap Life</h4>
                    <p style="margin-bottom:5px">Era Swap Life is Single Sign On (SSO) gateway which provides access to multiple utility platforms accepting Era Swap (ES) Utility token.</p>
                    <a style="text-decoration:underline;" href="https://www.youtube-nocookie.com/embed/WTydWvczn8g?rel=0&amp" target="_blank" rel="noopenner noreferrer">View Era Swap Teaser</a>


                <div class="col-xl-3 col-lg-4 col-md-6 col-12">
                    <div class="single-widget widget-recentpost">
                    </div>
                </div>
            </div>
        </div>
    </div>
            <!-- <div class="row">
                <div class="col-md-12 offset-md-5">
                    <h4 class="text-white">Smart Contracts Address</h4><br>
                </div>
            </div>

            <div class="row offset-md-1" style="padding-bottom: 40px;">
                <div class="col-md-3">
                     <a href="https://etherscan.io/address/0xef1344bdf80bef3ff4428d8becec3eea4a2cf574#code" target="_blank" class="tm-button tm-button-lg tm-button-white tm-button-transparent" style="height: 41px; line-height: 36px"><i class="zmdi zmdi-apple"></i><span>Era Swap Token (ES)</span></a>
                </div>
                 <div class="col-md-3">
                     <a href="https://etherscan.io/address/0x20ee679d73559e4c4b5e3b3042b61be723828d6c#code" target="_blank" class="tm-button tm-button-lg tm-button-white tm-button-transparent" style="height: 41px; line-height: 36px"><i class="zmdi zmdi-apple"></i><span>Newly Released Token (NRT)</span></a>
                </div>
                 <div class="col-md-3">
                     <a href="https://etherscan.io/address/0x42225682113E6Ed3616B36B4A72BbaE376041D7c#code" target="_blank" class="tm-button tm-button-lg tm-button-white tm-button-transparent" style="height: 41px; line-height: 36px"><i class="zmdi zmdi-apple"></i><span>BetDeEx DApp</span></a>
                </div>
                 <div class="col-md-3">
                     <a href="https://etherscan.io/address/0x5630ee5f247bd6b61991fbb2f117bbeb45990876#code" target="_blank" class="tm-button tm-button-lg tm-button-white tm-button-transparent" style="height: 41px; line-height: 36px"><i class="zmdi zmdi-apple"></i><span>TimeAlly DApp</span></a>
                </div>
                <div class="col-md-3">
                     <a href="https://etherscan.io/address/0xbad9af4db5401b7d5e8177a18c1d69c35fc03fd3#code" target="_blank" class="tm-button tm-button-lg tm-button-white tm-button-transparent" style="height: 41px; line-height: 36px"><i class="zmdi zmdi-apple"></i><span>TSGAP DApp</span></a>
                </div>
            </div> -->
        <div class="tm-footer-bottomarea">
            <div class="row align-items-center">
                <div class="col-md-9">
                    <p class="tm-footer-copyright">Copyright 2020 <a rel="noopener noreferrer">Era Swap Life</a>. </p>
                    <ul class="policy_list">
                        <li><a href="/#about_us_1">About us</a> |</li>
                        <li><a href="sitemap.html" target="_blank">Sitemap</a> |</li>
                        <li><a href="/pdf/EraSwapLife_Terms&Service.pdf" target="_blank">Terms and conditions</a> |</li>
                        <li><a href="/pdf/EraSwapLife_Privacy.pdf" target="_blank">Privacy policy</a></li>

                    </ul>
                </div>
                <!-- <div class="col-md-5">
                            <ul class="tm-footer-social">
                                <li>
                                    <a href="#">
                                        <i class="zmdi fa fa-facebook"></i>
                                        <i class="zmdi zmdi-facebook"></i>
                                    </a>
                                </li>
                                <li>
                                    <a href="#">
                                        <i class="zmdi zmdi-dribbble"></i>
                                        <i class="zmdi zmdi-dribbble"></i>
                                    </a>
                                </li>
                                <li>
                                    <a href="#">
                                        <i class="zmdi zmdi-behance"></i>
                                        <i class="zmdi zmdi-behance"></i>
                                    </a>
                                </li>
                                <li>
                                    <a href="#">
                                        <i class="zmdi zmdi-twitter"></i>
                                        <i class="zmdi zmdi-twitter"></i>
                                    </a>
                                </li>
                                <li>
                                    <a href="#">
                                        <i class="zmdi zmdi-pinterest"></i>
                                        <i class="zmdi zmdi-pinterest"></i>
                                    </a>
                                </li>
                            </ul>
                        </div> -->

            </div>
        </div>
    </div>
</div>
<!--// Footer Area -->
