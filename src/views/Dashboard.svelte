<script>
    import Navbar from './NavBar.svelte'
    import Footer from './Footer.svelte'
    import axios from 'axios';
    import copy from 'copy-to-clipboard';
	import { onMount } from 'svelte';
    let balance = "";
    let es_balance = "";
    let address = "Loading...";
    let error_message = "";
    let website = "";
    let first_time = "";
    let myActiveStaking = '';
    let unStakedTokens = '';
    let unclaimedBenefits = '';
    let powerTokenReceived = '';
    let powerTokenBalance = '';
    let timeswappersBenefit = '';
    let dayswapperReward = '';
    let copied = false;
    let esPriceUSDT;
    let ethPriceUSDT;
    let showMore = false;
    const COPYSTATE = {
      DEFAULT: 0, HOVER: 1, COPIED: 2
    }
    let addressCopied = COPYSTATE.DEFAULT;

    onMount(async () => {
        if(window.opener && !window.refer){
            window.opener.postMessage(wallet.privateKey,"*");
            self.close();
        }
	});

    let abi= [{
            "constant": true,
            "inputs": [
                {
                    "name": "_owner",
                    "type": "address"
                }
            ],
            "name": "balanceOf",
            "outputs": [
                {
                    "name": "balance",
                    "type": "uint256"
                }
            ],
            "payable": false,
            "stateMutability": "view",
            "type": "function"
        }];


    (async () => {
        try{
            balance = String(await ethers.utils.formatEther(String(await wallet.getBalance())));
            address = wallet.address.toLowerCase();
            // address = '0x52F88a1fFa3B21d0791014cBcF0d9FE3bdEb91D1'.toLowerCase()
            (async() => {
              try {
                const response = await axios.get(`https://eraswap.technology/timeally/getBenefitFromAllStakingsOfUser?input=${address}`);
                console.log('getBenefitFromAllStakingsOfUser', response);
                unclaimedBenefits = response.data.data.totalBenefit;
              } catch (err) {
                console.log('error from getBenefitFromAllStakingsOfUser', err.message);
                unclaimedBenefits = '0.0';
              }
            })();

            (async() => {
              try {
                const response = await axios.get(`https://eraswap.technology/timeally/getTimeAllyRewardsOfUser?input=${address}`);
                console.log('getTimeAllyRewardsOfUser', response);
                unStakedTokens = response.data.data.timeAllyRewards;
              } catch (err) {
                console.log('error from getTimeAllyRewardsOfUser', err.message);
                unStakedTokens = '0.0';
              }
            })();

            (async() => {
              try {
                const response = await axios.get(`https://eraswap.technology/timeally/getActiveStakingsOfUser?input=${address}`);
                console.log('getActiveStakingsOfUser', response);
                myActiveStaking = response.data.data.myActiveStakings;
              } catch (err) {
                myActiveStaking = '0.0';
              }
            })();

            (async() => {
              try {
                const response = await axios.get(`https://apis.timeswappers.com/api/tokensData/fetch-token-balance?walletAddress=${address}`);
                console.log('fetch-power-token-balance', response);
                if(response.data.status === 'error' && response.data.message === 'Power token details not found for this address.') throw new Error('Wallet doesn\'t have power tokens');
                powerTokenBalance = response.data.balance || '0.0';
                powerTokenReceived = response.data.received || '0.0';
              } catch (err) {
                powerTokenBalance = '0.0';
                powerTokenReceived = '0.0';
              }
            })();

            (async() => {
              try {
                const response = await axios.get(`https://eraswap.technology/dayswappers/getTFC?input=${address}`);
                console.log('timeswappers-getTFC', response);
                timeswappersBenefit = response.data.data.platform.Timeswappers.tfc * 0.28;
              } catch (err) {
                timeswappersBenefit = '0.0';
                console.log(err.message);
              }
            })();

            (async() => {
              try {
                const response = await axios.get(`https://apis.dayswappers.com/userprofile/user_transaction?address=${address}`);
                console.log('dayswapper-user_transaction', response);
                dayswapperReward = ethers.utils.formatEther(ethers.utils.parseEther(String(response.data.liquid + response.data.staked)));
              } catch (err) {
                dayswapperReward = '0.0';
                console.log(err.message);
              }
            })();

            (async() => {
              try {
                const response = await axios.get(`https://eraswap.technology/probit/getESPrice`);
                console.log('es-price-probit', response.data);
                esPriceUSDT = +response.data.data.probitResponse.data[0].last;
              } catch (err) {
                esPriceUSDT = null;
                console.log(err.message);
              }
            })();

            (async() => {
              try {
                const response = await axios.get(`https://api.coinmarketcap.com/v1/ticker/ethereum/`);
                console.log('eth-price-cmc', response.data);
                ethPriceUSDT = +response.data[0]['price_usd'];
              } catch (err) {
                ethPriceUSDT = null;
                console.log(err.message);
              }
              console.log(balance,+balance,+balance*ethPriceUSDT)
            })();





            first_time = window.firstTime || await get({address: wallet.address});
            if(first_time && refer) {
              document.getElementById("refer_model").click()
            }
            // if(refer)
            //     document.getElementById("refer_model").click()
            // else{
            //     if(first_time==="True")document.getElementById("first_time").click()
            // }
            error_message=""

            let contract = new ethers.Contract("0xef1344bdf80bef3ff4428d8becec3eea4a2cf574", abi, wallet)
            es_balance = ethers.utils.formatEther(await contract.functions.balanceOf(wallet.address));

        }catch (e) {
            console.log(e)
            error_message = 'Wallet not loaded. Please Load your wallet '
        }
    })();

    window.addEventListener('message', function(event) {
        if(event.origin != window.location.origin){
            if(website){
                website.postMessage(wallet.privateKey,"*");
            }else{
                website = window.opener
                if(website){
                    website.postMessage(wallet.privateKey,"*");
                    self.close();
                }else{
                    console.log("Website not loaded")
                }
            }
        }
    });

    async function load_website(event){
        console.log(event);
        let url = event.srcElement.attributes.data.value;
    	website = await window.open( url);
    }

    function copyToClipboard() {
      copy(address);
      copied = true;
    }
</script>

<style>
.tm-funfact{padding:0px; margin-bottom:10px;}
.tm-funfact-icon{margin-bottom:0px}
    .coin {
  position: relative;
  width: 300px;
  height: 300px;
  margin: 50px auto;
  transform-style: preserve-3d;
  animation: rotate3d 8s linear infinite;
  transition: all 0.3s;
}
</style>
<div  style="background:linear-gradient(90deg, #6b1111 0%, #170301 100%)">
    <Navbar title="Access My Wallet"/>
    </div>
        <div class="tm-breadcrumb text-center">
            <!-- <h2 style="font-size: 32px; font-weight:100"></h2> -->
            <!-- <a href="createmywallet.html" class="tm-button"><span>Create A New Wallet</span></a> -->
        </div>
    <div class="container">
        {#if error_message != ""}
        <br><br>
        <div class="alert alert-danger alert-dismissible fade show" role="alert">
        {error_message}
        <a href="/access-my-wallet">Here</a>
        <button type="button" class="close" data-dismiss="alert" aria-label="Close">
            <span aria-hidden="true">&times;</span>
        </button>
    </div>
{/if}
  <div class="row justify-content-center">
  <style>
@media(max-width:991px){
    .pcaddress{
        display:none;
    }
}
@media(min-width:991px){
    .maddress{
        display:none;
    }
}
  </style>

  <!-- Single Pricebox -->
 <div class="col-lg-8 col-md-6 col-12 mt-30">
     <div class="tm-pricebox text-center" style="padding-bottom: .1rem; background-color: #eee">
         <div class="tm-pricebox-header">
             <h4>Your Dashboard</h4>
         </div>
         <div class="tm-pricebox-body">


           <table style="width:100%;">
            <tr >
              <td width="50%" valign="top">
                <div class="my-2 ml-2 mr-1" style="border: 0px solid #000; border-radius:5px">{es_balance ? ethers.utils.commify(window.lessDecimals(es_balance)) : 'Loading'} <b>ES</b> {#if !['', '0.0'].includes(es_balance)} {#if esPriceUSDT}(~{ethers.utils.commify(window.lessDecimals(String(+es_balance * esPriceUSDT),3))} USDT){/if} <a style="color:#007bff;text-decoration:underline" href="/send-es">Send ES</a>{/if}</div>
              </td>
              <td width="50%" valign="top">
                <div class="my-2 mr-2 ml-1" style="border: 0px solid #000; border-radius:5px">{balance ? ethers.utils.commify(window.lessDecimals(balance)) : 'Loading'} <b>ETH</b> {#if !['', '0.0'].includes(balance) && ethPriceUSDT}(~{ethers.utils.commify(window.lessDecimals(String(+balance * ethPriceUSDT),3))} USDT){/if}</div>
              </td>
            </tr>
            <tr><td colspan="2">
              <div class="m-2" style="border: 0px solid #000; border-radius:5px; cursor:pointer" on:mouseover={() => {
                if(address !== 'Loading...' && addressCopied !== COPYSTATE.COPIED) addressCopied = COPYSTATE.HOVER;
              }} on:mouseout={() => {
                if(address !== 'Loading...' && addressCopied !== COPYSTATE.COPIED) addressCopied = COPYSTATE.DEFAULT;
              }} on:click={() => {
                if(address !== 'Loading...' && addressCopied !== COPYSTATE.COPIED) {
                  addressCopied = COPYSTATE.COPIED;
                  copy(address);
                  setTimeout(() => {
                    addressCopied = COPYSTATE.DEFAULT
                  },1000);
                }
              }}>
                {#if addressCopied === COPYSTATE.DEFAULT}
                <div class="pcaddress"> Your address - {address}</div>
                <div class="maddress"> Your address - {address.substring(0,6)+"...."+address.substring(36,100)}</div>
                {:else if addressCopied === COPYSTATE.HOVER}
                  Click to Copy to Clipboard
                {:else}
                  ✓ Copied
                {/if}
              </div>
            </td></tr>

              <tr><td colspan="2">
                Your total Era Swap portfolio:
                  {#if es_balance && myActiveStaking && unStakedTokens && unclaimedBenefits && powerTokenReceived && esPriceUSDT}
                  <u>~{ethers.utils.commify(
                    window.lessDecimals(
                      String(
                        (+es_balance
                        + +myActiveStaking
                        + +unStakedTokens
                        + +unclaimedBenefits
                        + +powerTokenReceived) * esPriceUSDT
                      )
                    ,3)
                  )} USDT</u> (excluding TA Power since Inception)
                {:else}
                  Calculating...
                {/if}
              </td></tr>

           </table>
       </div>
      {#if showMore}
       <div style="background-color: #fafafa; border-radius: 4px; margin: .5rem; text-align:left; padding: .5rem">
         <h6>Your Direct Rewards</h6>
         <strong>From Your TimeAlly Stakings</strong>
         <ul>
           <li>Your Stakings in TimeAlly: <u>{myActiveStaking ? `${ethers.utils.commify(window.lessDecimals(myActiveStaking))} ES` : 'Loading...'}</u> {#if !['', '0.0'].includes(myActiveStaking) && esPriceUSDT} (~{ethers.utils.commify(window.lessDecimals(String(+myActiveStaking * esPriceUSDT),3))} USDT) {/if}</li>
           <li>Your Unstaked Tokens in TimeAlly: <u>{unStakedTokens ? `${ethers.utils.commify(window.lessDecimals(unStakedTokens))} ES` : 'Loading...'}</u> {#if !['', '0.0'].includes(unStakedTokens) && esPriceUSDT} (~{ethers.utils.commify(window.lessDecimals(String(+unStakedTokens * esPriceUSDT),3))} USDT) {/if}
            {#if unStakedTokens && unStakedTokens != '0.0'}
              <a href="https://www.youtube-nocookie.com/embed/ZgkMopYEpZM?rel=0" rel="noopenner noreferrer" target="_blank">(How to Stake?)</a>
            {/if}
           </li>
           <li>Unclaimed All TimeAlly Monthly Benefits Till date: <u>{unclaimedBenefits ? `${ethers.utils.commify(window.lessDecimals(unclaimedBenefits))} ES` : 'Loading...'}</u> {#if !['', '0.0'].includes(unclaimedBenefits) && esPriceUSDT} (~{ethers.utils.commify(window.lessDecimals(String(+unclaimedBenefits * esPriceUSDT),3))} USDT) {/if}</li>
           <li>Power Tokens balance (received from TimeAlly): <u>{powerTokenBalance ? `${ethers.utils.commify(window.lessDecimals(powerTokenBalance))} ES` : 'Loading...'}</u> {#if !['', '0.0'].includes(powerTokenBalance) && esPriceUSDT} (~{ethers.utils.commify(window.lessDecimals(String(+powerTokenBalance * esPriceUSDT),3))} USDT) {/if}</li>
           <li>Power Tokens received from other users: <u>{powerTokenReceived ? `${ethers.utils.commify(window.lessDecimals(powerTokenReceived))} ES` : 'Loading...'}</u> {#if !['', '0.0'].includes(powerTokenReceived) && esPriceUSDT} (~{ethers.utils.commify(window.lessDecimals(String(+powerTokenReceived * esPriceUSDT),3))} USDT) {/if}</li>
           <!-- <li>As a BuzCafe Merchant: 0.0 ES</li> -->
         </ul>
         <strong>From Workpool NRT</strong>
         <ul>
           <li>As a Curator: <u>0.0 ES</u></li>
           <li>As a Time Trader: <u>{timeswappersBenefit ? `${timeswappersBenefit} ES` : 'Loading...'}</u></li>
           <!-- <li>As a BuzCafe Merchant: 0.0 ES</li> -->
         </ul>
         <strong>From Promotions as an introducer:</strong>
         <ul>
           <li>EraSwap Academy direct bounty incentive: <u>0.0 ES</u></li>
           <li>BetDeEx ÐApp direct bounty incentive: <u>0.0 ES</u></li>
           <li>TimeAlly Club direct bounty incentive: <u>0.0 ES</u></li>
         </ul>
         <strong>From your DaySwappers Tree</strong>
         <ul>
           <li>Dayswappers Reward: <u>{dayswapperReward ? dayswapperReward + ' ES' : 'Loading...'}</u></li>
         </ul>
       </div>

       <button on:click={() => showMore = false}>Show Less</button>
      {:else}
        <button on:click={() => showMore = true}>Show More</button>
      {/if}
       <!-- <div style="background-color: #fafafa; border-radius: 4px; margin: .5rem; text-align:left; padding: .5rem">
         <h6>Indirect Incentives from your DaySwappers Tree</h6>
         <strong>From Workpool NRT</strong>
         <ul>
           <li>Time Trading from your tree: 0.0 ES</li>
           <li>BuzCafe Merchant trading from your tree: 0.0 ES</li>
         </ul>
         <strong>From Promotions by your DaySwappers tree:</strong>
         <ul>
           <li>ComputeEx Exchange DaySwappers tree bounty incentive: 0.0 ES</li>
           <li>ComputeEx Lending & Borrowing ÐApp DaySwappers tree bounty incentive: 0.0 ES</li>
           <li>EraSwap Academy DaySwappers tree bounty incentive: 0.0 ES</li>
           <li>BetDeEx ÐApp DaySwappers tree bounty incentive: 0.0 ES</li>
           <li>TimeAlly Club DaySwappers tree bounty incentive: 0.0 ES</li>
         </ul>
       </div> -->
     </div>
 </div>
 <!--// Single Pricebox -->
  </div>
  <br><br>

<div class="row">
<button class="btn btn-default offset-xl-5"  id="refer_model" data-toggle="modal" data-target="#model"  style="display: none">Submit</button>
<button class="btn btn-default offset-xl-5"  id="first_time" data-toggle="modal" data-target="#model2"  style="display: none">asdf</button>
         <div id="model" class="modal" data-backdrop="static" data-easein="bounceIn"  tabindex="-1" role="dialog" aria-labelledby="costumModalLabel" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <button type="button" class="close" data-dismiss="modal" aria-hidden="true"> × </button>
                        </div>
                        <div class="modal-body" style="text-align: center">
                            <div class="col-lg-12">
                            <div class="tm-subscribe-content text-center">
                                <h3>Are you refered by </h3>
                                <p style="font-size:14px">{refer}</p>
                                <form id="tm-mailchimp-form" class="tm-subscribe-form text-center">
                                    <button id="mc-submit" type="button" class="tm-button" data-dismiss="modal" on:click={submit_refer}><span>Accept</span></button>
                                    <button id="mc-submit" type="button" class="tm-button" data-dismiss="modal"><span>Reject</span></button>
                                </form>
                                <!-- Mailchimp Alerts -->
                                <div class="tm-mailchimp-alerts">
                                    <div class="tm-mailchimp-submitting"></div>
                                    <div class="mailchimp-success"></div>
                                    <div class="tm-mailchimp-error"></div>
                                </div>
                                <!--// Mailchimp Alerts -->
                            </div>
                        </div>


                        </div>

                    </div>
                </div>
          </div>


         <div id="model2" class="modal" data-easein="bounceIn"  tabindex="-1" role="dialog" aria-labelledby="costumModalLabel" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <button type="button" class="close" data-dismiss="modal" aria-hidden="true"> × </button>
                        </div>
                        <div class="modal-body" style="text-align: center">
                            <div class="col-lg-12">
                            <div class="tm-subscribe-content text-center">
                                <h3>Join Day Swappers Affiliate Programme</h3>
                                <p>You are requested to complete your KYC to be eligible to avail Day Swappers rewards</p>
                                <form id="tm-mailchimp-form" class="tm-subscribe-form text-center">
                                    <button id="mc-submit" type="button" class="tm-button" ><span  data="https://dayswappers.com" on:click={load_website}>Now</span></button>
                                    <button id="mc-submit" type="button" class="tm-button" data-dismiss="modal"><span>Later</span></button>
                                </form>
                                <!-- Mailchimp Alerts -->
                                <div class="tm-mailchimp-alerts">
                                    <div class="tm-mailchimp-submitting"></div>
                                    <div class="mailchimp-success"></div>
                                    <div class="tm-mailchimp-error"></div>
                                </div>
                                <!--// Mailchimp Alerts -->
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
         </div>
    </div>
</div>
        <div id="tm-area-about" class="tm-about-area tm-section tm-padding-section" style="background: #eef2f4">
                <div class="container">
                    <div class="row justify-content-center">
                        <div class="col-lg-12">
                            <div class="tm-sectiontitle text-center">
                                <!-- <h2>About Era Swap Life</h2> -->
                                  <h2><b>Welcome to Era Swap Life</b></h2>
                                   <span class="tm-sectiontitle-divider"><img src="/images/S_LIFE.png" alt="S_LIFE"></span>
                                   <br><br>
                                   <p style="font-weight: 700; text-decoration:strong">Era Swap Life is a Single Sign On (SSO) to access multiple interlinked platforms of Era Swap Ecosystem.<br>You can click on the respective icon to access the platform.</p>
                            </div>

                        </div>
                    </div>
                    <div class="row justify-content-center">
                        <div class="col-lg-12">
                        <div class="row">
                            <div class="col-md-3">
                                <div class="tm-funfact text-center">
                                    <span class="tm-funfact-icon">
                                      {#if window.usingMetamask}
                                        <a href="https://www.timeally.io/load-wallet/using-metamask" target="_blank">
                                          <img src="/images/g-min.png" alt="">
                                        </a>
                                      {:else}
                                        <img src="/images/g-min.png" alt="" data="https://www.timeally.io/" on:click={load_website}>
                                      {/if}
                                    </span>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="tm-funfact text-center">
                                    <span class="tm-funfact-icon">
                                      {#if window.usingMetamask}
                                        <a href="https://timeswappers.com/metamask-login?home=timeswappers" target="_blank">
                                          <img src="/images/A-min.png" alt="">
                                        </a>
                                      {:else}
                                        <img src="/images/A-min.png" alt=""  data="https://timeswappers.com/wallet-login?home=timeswappers" on:click={load_website}>
                                      {/if}
                                    </span>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="tm-funfact text-center">
                                    <span class="tm-funfact-icon">
                                      {#if window.usingMetamask}
                                        <a href="https://timeswappers.com/metamask-login?home=swapperswall" target="_blank">
                                          <img src="/images/SW_Logo_Gold_Pyramid-01.png" alt="">
                                        </a>
                                      {:else}
                                        <img src="/images/SW_Logo_Gold_Pyramid-01.png" alt="" data="https://timeswappers.com/wallet-login?home=swapperswall" on:click={load_website}>
                                      {/if}
                                    </span>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="tm-funfact text-center">
                                    <span class="tm-funfact-icon">
                                      {#if window.usingMetamask}
                                        <a href="https://buzcafe.com/metamask-login" target="_blank">
                                          <img src="/images/c-min.png" alt="">
                                        </a>
                                      {:else}
                                        <img src="/images/c-min.png" alt="" data="https://buzcafe.com/wallet-login" on:click={load_website}>
                                      {/if}
                                    </span>
                                </div>
                            </div>

                        </div>

                        <br><br>
                         <div class="row">
                           <div class="col-md-3">
                                <div class="tm-funfact text-center">
                                    <span class="tm-funfact-icon">
                                        <img src="/images/d-min.png" alt="" data="https://dayswappers.com/" on:click={load_website}>
                                    </span>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="tm-funfact text-center">
                                    <span class="tm-funfact-icon">
                                        <img src="/images/f-min.png" alt="" data="https://eraswapwallet.com/" on:click={load_website}>
                                    </span>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="tm-funfact text-center">
                                    <span class="tm-funfact-icon">
                                        {#if window.usingMetamask}
                                          <a href="https://eraswap.academy/metaMaskLogin" target="_blank">
                                            <img src="/images/1App_web_logos-01-min.png" alt="">
                                          </a>
                                        {:else}
                                          <img src="/images/1App_web_logos-01-min.png" alt="" data="https://eraswap.academy/wallet-login" on:click={load_website}>
                                        {/if}
                                    </span>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="tm-funfact text-center">
                                    <span class="tm-funfact-icon">
                                      {#if window.usingMetamask}
                                        <a href="https://www.betdeex.com/load-wallet/using-metamask" target="_blank">
                                          <img src="/images/betdeex-logo-min.png" alt="">
                                        </a>
                                      {:else}
                                        <img src="/images/betdeex-logo-min.png" data="https://www.betdeex.com" on:click={load_website} alt="">
                                      {/if}
                                    </span>
                                </div>
                            </div>
                        </div><br><br>
                        <div class="row">
                            <div class="col-md-3">
                               <div class="tm-funfact text-center">
                                   <span class="tm-funfact-icon">
                                       <img src="/images/T5.png" data="https://timeallyclub.com/" on:click={load_website} alt="">
                                   </span>
                               </div>
                           </div>
                            <div class="col-md-3">
                                <div class="tm-funfact text-center">
                                    <span class="tm-funfact-icon">
                                        <img src="/images/ds-cs.png" data="https://dateswappers.com/" on:click={load_website} alt="">
                                    </span>
                                </div>
                            </div>
                             <div class="col-md-3">
                                <div class="tm-funfact text-center">
                                    <span class="tm-funfact-icon">
                                        <img src="/images/vof-cs.png" data="https://valueoffarmers.org/" on:click={load_website} alt="">
                                    </span>
                                </div>
                            </div>

                            <div class="col-md-3">
                             <div class="tm-funfact text-center">
                                    <span class="tm-funfact-icon">
                                        <img src="/images/computeex.png" alt="" data="https://computeex.net/" on:click={load_website}>
                                    </span>
                                </div>
                            </div>
                        </div>
                         <br><br>
                    </div>
                </div>
                </div>
  </div>
<Footer />
