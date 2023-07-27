

/*
 * @Author: xz
 * @Date: 2021-04-14 14:35:16
 * @LastEditTime: 2022-04-28 16:35:39
 * @LastEditors: xz
 * @Description: 
 */

var CoinApp = (function () {
    function CoinApp() {
        if (CoinApp.instance) {
            console.log("can not new App again");
        }
    };

    /**
     * @description 分享
     * @param text 分享文字，可以传入本身游戏的游戏名称，或者一句好玩的英文
     * @param image 分享图的base64 ,默认可以用物料图中的1200*627
    */
    CoinApp.shareAsync = async function (text = "Smile Game", image = window.fbShareCfg.share_icon) {
        // if (!FBInstant) {
        //     return;
        // }
        // FBInstant.shareAsync({
        //     image: image,
        //     text: text,
        //     data: { myReplayData: '...' },
        //     switchContext: false,
        // }).then(function () {
        //     // continue with the game.
        // });
    }

    /**
     * @description 创建快捷方式异步 安卓手机特有
    */
    CoinApp.createShortcutAsync = async function () {
        // if (!FBInstant) {
        //     return;
        // }
        // if (!FBInstant.canCreateShortcutAsync) {
        //     console.log("环境不支持 创建快捷方式 canCreateShortcutAsync");
        //     return;
        // }
        // FBInstant.canCreateShortcutAsync()
        //     .then(function (canCreateShortcut) {
        //         if (canCreateShortcut) {
        //             FBInstant.createShortcutAsync()
        //                 .then(function () {
        //                     // Shortcut created
        //                 })
        //                 .catch(function () {
        //                     // Shortcut not created
        //                 });
        //         }
        //     });
    }

    /**
     * @description 清楚存储数据
    */
    CoinApp.clear = async function () {
        await window.FBInstant.player.setDataAsync({}).then(function () {
            console.log('data is clear');
        });
    }

    /**
     * @description 设置存储数据
     * @param _key 
     * @param _str
    */
    CoinApp.setItem = async function (_key, _str) {
        let playerData = window.localStorage.getItem("playerData") || "{}";
        playerData = JSON.parse(playerData);

        let tempObject = {};
        tempObject[_key] = _str;
        let object = Object.assign(playerData, tempObject);

        window.localStorage.setItem("playerData",JSON.stringify(object));

        // await window.FBInstant.player.setDataAsync(object).then(function () {
        //     console.log('data is set');
        // });
    }

    /**
     * @description 关键数据立即刷新
    */
    CoinApp.flushDataAsync = async function () {
        // await FBInstant.player.flushDataAsync();//刷新数据
    }

    /**
     * @description 获取存储数据
     * @param key 
    */
    CoinApp.getItem = async function (key) {
        //let data = await FBInstant.player.getDataAsync([key]);

         let playerData = window.localStorage.getItem("playerData") || "{}";
         playerData = JSON.parse(playerData);

         // playerData[key] =  data[key];

         window.localStorage.setItem("playerData",JSON.stringify(playerData));

        return playerData[key];
    }

    /**
     * @description 删除存储数据
     * @param key 
    */
    CoinApp.removeItem = async function (key) {
        await CoinApp.setItem(key, "");
    }

    /**
    * @description 获取存储数据 json
    * @param key 
   */
    CoinApp.getJSON = async function (key) {
        let data = await CoinApp.getItem(key);
        if (typeof data == "string") {
            data = JSON.parse(data);
        }
        return data;
    }

    /**
     * @description 设置存储数据 json
     * @param _key 
     * @param _str
    */
    CoinApp.setJSON = async function (key, _str) {
        if (typeof _str == "object") {
            _str = JSON.stringify(_str);
        }
        await CoinApp.setItem(key, _str);
    }

    /**
     * @description fb 模拟加载
    */
    CoinApp.startShowLoading = async function (key) {
        let i = 1;
        let max = 99;
       // FBInstant.setLoadingProgress(1);
        window.progressTimer = setInterval(() => {
            i++;
           // FBInstant.setLoadingProgress(i / 100 * 100);
            i = Math.min(max, i);
            if (i == max) {
                clearInterval(window.progressTimer);
                window.progressTimer = null;
            }
        }, 1000);
    }



    /**
     * @description fb游戏开始
    */
    CoinApp.onStart = function () {
        if (CoinApp.isOnStarted) {
            //已经初始化过
            return;
        }
        CoinApp.isOnStarted = true;

        if (window['progressTimer']) {
            clearInterval(window.progressTimer);
            window.progressTimer = null;
        }

        // if (!!window['FBInstant']) {
        //     window['FBInstant'].setLoadingProgress(100);
        //     window['FBInstant'].startGameAsync().then(() => {
        //         console.log("fb游戏开始");
        //         console.log('player=getPhoto===', FBInstant.player.getPhoto());
        //         console.log('player==getName==', FBInstant.player.getName());
        //         console.log('player=getID===', FBInstant.player.getID());
        //         console.log('==context=getType=', FBInstant.context.getType());
        //         console.log('==context=getID=', FBInstant.context.getID());

        //         CoinApp.isOpenRank && CoinApp.fetchLeaderboard();
        //         CoinApp.initAd && CoinApp.initAd();
        //     }).catch(function (b) {
        //         console.log(b.message);
        //     });
        // }
    }

    //插屏广告
    CoinApp.loadedAd = null;
    CoinApp.preLoadRunning = !1;
    CoinApp.retryTime = 32000;
    CoinApp.retryCount = 0;
    CoinApp.stopRetry = !1;
    CoinApp.stopBeHandle = !1;
    CoinApp.hasRemoveAD = !1;

    /**
     * @description 一进入游戏就预加载插屏广告，这种模式，需要后面有自动弹出插屏的机会，不然会使得展示率很低
    */
    CoinApp.startPreLoad = function () {
        // if (!FBInstant.getInterstitialAdAsync) {
        //     return;
        // }
        // CoinApp.hasRemoveAD || (setInterval(CoinApp.checkHasAd, CoinApp.retryTime),
        //     CoinApp.checkHasAd())
    }


    CoinApp.checkHasAd = function () {
        CoinApp.stopBeHandle ? CoinApp.stopBeHandle = !1 : null != CoinApp.loadedAd || CoinApp.stopRetry || CoinApp.preLoadAd()
    }

    CoinApp.preLoadAd = function () {
        // if (!CoinApp.preLoadRunning && !CoinApp.loadedAd) {
        //     CoinApp.preLoadRunning = !0;
        //     var a = CoinApp, b;
        //     FBInstant.getInterstitialAdAsync(CoinApp.AD_ID_INTER).then(function (c) {
        //         console.log("ready pre load AD");
        //         b = c;
        //         return b.loadAsync()
        //     }).then(function () {
        //         console.log("Rewarded video preloaded");
        //         a.loadedAd = b;
        //         a.preLoadRunning = !1
        //     }).catch(function (b) {
        //         console.log("Rewarded video failed to preload: " + b.message);
        //         console.log("code", b.code);
        //         a.preLoadRunning = !1;
        //         a.retryCount++;
        //         3 <= a.retryCount && (a.stopRetry = !0,
        //             a.stopBeHandle = !0)
        //     })
        // }
    }

    /**
     * @description  两次插屏广告之间的弹出间隔
     */
    CoinApp.showInterstitialTimeDelt = 60000;

    /**
     * @description fb 显示插屏广告
     */
    CoinApp.showInterstitial = function (complete) {
        console.log("本地测试 --- showInterstitial");
		console.log("广告场景:插屏");
        //如果是送审版 ***************游戏内自行完善***************
        CoinApp.beforeShowAd();
        if (!CoinApp.isEnableAD) {
			ENF_showInterstitialAd(()=> {
                complete && complete();
                CoinApp.afterShowAd();
            }, ()=>{
                complete && complete();
                CoinApp.afterShowAd();
            });
        }

        // if (window.location && window.location.href && window.location.href.indexOf("localhost") != -1) {
        //     complete && complete();
        //     complete = null;
        //     return;
        // }

        // if (CoinApp.hasRemoveAD) {
        //     complete && complete();
        //     complete = null;
        // }
        // else {
        //     //两次插屏广告之间的弹出间隔不得<60秒 ***************游戏内自行完善***************
        //     var now = Date.parse(new Date());
        //     if (CoinApp.showInterTimestamp) {
        //         var off = now - CoinApp.showInterTimestamp;
        //         if (off <= CoinApp.showInterstitialTimeDelt) {
        //             console.log('时间少于x秒，不弹出插屏广告！！！');
        //             complete && complete();
        //             complete = null;
        //             return;
        //         }
        //     }
        //     CoinApp.showInterTimestamp = now;

        //     //是否需要现用现加载 ****************游戏内自行完善***************
        //     if (CoinApp.isNeedLoadInterstitial) {
        //         CoinApp.addLoadingCircle();
        //         //开始游戏时，没有预加载使用这种模式
        //         let ad_ins = null;
        //         FBInstant.getInterstitialAdAsync(CoinApp.AD_ID_INTER)
        //             .then(function (temp) {
        //                 // Load the Ad asynchronously
        //                 console.log("插屏广告 成功获得实例");
        //                 ad_ins = temp;
        //                 return ad_ins.loadAsync();
        //             }).then(function () {
        //                 console.log("插屏广告 加载成功回调");
        //                 CoinApp.beforeShowAd();
        //                 return ad_ins.showAsync()
        //             })
        //             .then(function () {
        //                 console.log("插屏广告 广告显示成功");
        //                 CoinApp.removeLoadingCircle();
        //                 CoinApp.afterShowAd();
        //                 complete && complete();
        //                 complete = null;
        //             }).catch(function (b) {
        //                 console.log(b.message);
        //                 console.log("插屏广告 广告显示失败-----------1");
        //                 CoinApp.removeLoadingCircle();
        //                 CoinApp.afterShowAd();
        //                 complete && complete();
        //                 complete = null;
        //             })
        //     } else {
        //         //游戏中，每关有自动弹出的使用这种模式，要配合开始游戏做预加载
        //         CoinApp.stopRetry = !1;
        //         if (CoinApp.loadedAd) {
        //             CoinApp.beforeShowAd();
        //             CoinApp.loadedAd.showAsync().then(function () {
        //                 console.log("InterstitialAd 广告显示成功");
        //                 CoinApp.loadedAd = null;
        //                 CoinApp.stopBeHandle = !0;
        //                 CoinApp.afterShowAd();
        //                 complete && complete();
        //                 complete = null;
        //             }).catch(function (c) {
        //                 CoinApp.loadedAd = null;
        //                 console.log("InterstitialAd 广告显示失败");
        //                 console.log(c.code, c.message);
        //                 CoinApp.stopBeHandle = !0;
        //                 CoinApp.afterShowAd();
        //                 complete && complete();
        //                 complete = null;
        //             })
        //         } else {
        //             console.log("RewardedVideo 广告未加载好");
        //             CoinApp.afterShowAd();
        //             complete && complete();
        //             complete = null;
        //         }
        //     }
        // }
    }
    /**
    * @description fb log
    */
    CoinApp.logEvent = function (a, b, c) {
        console.log("打点:", a, b, c);
        // FBInstant.logEvent(a, b, c);
    }

    /**
     * @description 激励视频看了几次统计
    */
    CoinApp.watchNum = 0;

    /**
     * @description 两次插屏广告之间的弹出间隔
    */
    CoinApp.showRewardTimeDelt = 2000;

    /**
     * @description fb 显示激励视频
     */
    CoinApp.showRewarded = function (success = null, failure = null) {
        console.log("本地测试 --- showRewarded");
		console.log("广告场景");
        //如果是送审版 ***************游戏内自行完善***************
        if (!CoinApp.isEnableAD) {
			 CoinApp.beforeShowAd();
			ENF_showRewardedVideoAd(() => {            
                success && success();
                success = null;
				CoinApp.afterShowAd()
            },(event) => {
                if (failure) {
                    failure();
                    failure = null;
                }
                // promptMessage("No ads temporarily");
				CoinApp.afterShowAd()
            }); 
            return;
        }

        // if (window.location && window.location.href && window.location.href.indexOf("localhost") != -1) {
        //     success && success();
        //     success = null;
        //     return;
        // }

        // //两次激励视频广告之间的弹出间隔不得<x秒  ***************游戏内自行完善***************
        // var now = Date.parse(new Date());
        // if (CoinApp.showAdRewardTimestamp) {
        //     var off = now - CoinApp.showAdRewardTimestamp;
        //     if (off <= CoinApp.showRewardTimeDelt) {
        //         console.log('不弹出banner广告！！！');
        //         return;
        //     }
        // }
        // CoinApp.showAdRewardTimestamp = now;

        // CoinApp.addLoadingCircle();

        // var d = null;
        // FBInstant.getRewardedVideoAsync(CoinApp.AD_ID_REWARD).then(function (a) {
        //     console.log("RewardedVideo  加载成功回调");
        //     d = a;
        //     return d.loadAsync()
        // }).then(function () {
        //     console.log("RewardedVideo 加载成功回调");
        //     CoinApp.beforeShowAd();
        //     return d.showAsync()
        // }).then(function () {
        //     console.log("RewardedVideo 广告显示成功");
        //     CoinApp.removeLoadingCircle();
        //     CoinApp.afterShowAd();
        //     CoinApp.watchNum += 1;
        //     success && success();
        //     success = null;
        // }).catch(function (b) {
        //     console.log(b.message);
        //     console.log("RewardedVideo 广告显示失败-----------1");
        //     CoinApp.removeLoadingCircle();
        //     CoinApp.afterShowAd();

        //     if (b.message) {
        //         CoinApp.ShowXZMessage(b.message + ",Please try again later!");
        //     }

        //     failure && failure();
        //     failure = null;

        //     console.log("RewardedVideo 广告显示失败------------2")
        // })
    }


    /**
     * 转圈圈 显示
    */
    CoinApp.addLoadingCircle = function () {
        if (window.Laya) {
            if (!CoinApp.circleLayer) {
                CoinApp.circleLayer = new Laya.Box();
                CoinApp.circleLayer.name = "circleLayer";
                CoinApp.circleLayer.width = 750;
                CoinApp.circleLayer.height = 2000;
                CoinApp.circleLayer.anchorX = 0.5;
                CoinApp.circleLayer.anchorY = 0.5;
                CoinApp.circleLayer.x = Laya.stage.width / 2;
                CoinApp.circleLayer.y = Laya.stage.height / 2;
                CoinApp.circleLayer.on(Laya.Event.CLICK, null, () => { });
                CoinApp.circleLayer.zOrder = 9999;
                Laya.stage.addChild(CoinApp.circleLayer);

                let bg = new Laya.Image();
                bg.skin = "coinsdk/black.png";
                bg.anchorX = 0.5;
                bg.anchorY = 0.5;
                bg.width = 750;
                bg.height = 3000;
                bg.x = Laya.stage.width / 2;
                bg.y = Laya.stage.height / 2;
                bg.alpha = 0.3;
                CoinApp.circleLayer.addChild(bg);

                let circle_img = new Laya.Image();
                circle_img.skin = "coinsdk/img-loading.png";
                circle_img.anchorX = 0.5;
                circle_img.anchorY = 0.5;
                circle_img.centerX = 0;
                circle_img.x = Laya.stage.width / 2;
                circle_img.y = Laya.stage.height / 2 + 300;
                CoinApp.circleLayer.addChild(circle_img);

                let time = 1000;
                circle_img.rotation = 0;
                let tl = Laya.TimeLine.to(circle_img, { rotation: 360 }, time);
                tl.play(0, true);
            }

            CoinApp.circleLayer.visible = true;
        } else {
            var gifloading = document.getElementById("gifloading");
            if (gifloading) {
                gifloading.style.display = 'block'
            }
        }
    }

    /**
     * 转圈圈 隐藏
    */
    CoinApp.removeLoadingCircle = function () {
        if (window.Laya) {
            if (CoinApp.circleLayer) {
                CoinApp.circleLayer.visible = false;
            }
        } else {
            var gifloading = document.getElementById("gifloading");
            if (gifloading) {
                gifloading.style.display = 'none'
            }
        }
    }

    /**
    * @description  两次banner广告之间的弹出间隔
    */
    CoinApp.showBannerTimeDelt = 30000;

    /**
    * @description  显示banner
    */
    CoinApp.showBanner = function (success, failure) {
        console.log("本地测试 --- showBanner");
		console.log("广告场景:Banner");
        // if (!FBInstant) {
        //     return;
        // }
        // if (!FBInstant.loadBannerAdAsync) {
        //     return;
        // }

        // if (!CoinApp.AD_ID_BANNER) {
        //     return;
        // }

        // //两次banner广告之间的弹出间隔不得<x秒  ***************游戏内自行完善***************
        // var now = Date.parse(new Date());
        // if (CoinApp.showAdBannerTimestamp) {
        //     var off = now - CoinApp.showAdBannerTimestamp;
        //     if (off <= CoinApp.showBannerTimeDelt) {
        //         console.log('不弹出banner广告！！！');
        //         return;
        //     }
        // }
        // CoinApp.showAdBannerTimestamp = now;


        // FBInstant.loadBannerAdAsync(
        //     CoinApp.AD_ID_BANNER
        // ).then(() => {
        //     console.log('success banner');
        // }).catch(function (error) {

        //     console.log(error.message);
        // });
    }

    /**
    * @description fb 隐藏banner
    */
    CoinApp.hideBanner = function (success, failure) {
        console.log("本地测试 --- hideBanner");
        // if (!FBInstant) {
        //     return;
        // }
        // if (!FBInstant.hideBannerAdAsync) {
        //     return;
        // }

        // FBInstant.hideBannerAdAsync();
    }

    /**
    * @description 看广告前的游戏处理
    */
    CoinApp.beforeShowAd = function () {
        if (window.Laya) {
            Laya.timer.scale = 0;//时间暂停
            Laya.stage.renderingEnabled = false; //停止渲染
            Laya.updateTimer && Laya.updateTimer.pause(); //停止onUpdate
            Laya.physicsTimer && Laya.physicsTimer.pause(); //停止物理
            
            window.WebAudioEngine && (window.WebAudioEngine.muted = true);
            Laya.SoundManager.setMusicVolume(0);
            Laya.SoundManager.setSoundVolume(0);
            Laya.SoundManager.muted = true;
        }
    }
    /**
    * @description 看广告后的游戏处理
    */
    CoinApp.afterShowAd = function () {
        if (window.Laya) {
            window.focus(); //找回焦点
            Laya.timer.scale = 1;//时间恢复
            Laya.stage.renderingEnabled = true //恢复渲染
            Laya.updateTimer && Laya.updateTimer.resume() //恢复onUpdate
            Laya.physicsTimer && Laya.physicsTimer.resume() //恢复物理

            //背景音乐还原 ***************游戏内自行完善***************
            Laya.SoundManager.setMusicVolume(1);
            window.WebAudioEngine && (window.WebAudioEngine.muted = false);
            Laya.SoundManager.setSoundVolume(1);
            Laya.SoundManager.musicMuted = false;
        }
    }
    /**
    * @description 提示 实现部分，***************游戏内自行完善***************
    */
    CoinApp.ShowXZMessage = function (tip = "") {
        if (tip) {
            if (window.ShowXZMessage) {
                window.ShowXZMessage(tip);
            } else {
                CoinApp.showToast(tip);
            }
        }
    }

    /**
     * 飘字提示
    */
    CoinApp.showToast = function (tip = "") {
        if (window.Laya) {
            if (!CoinApp.toastLayer) {

                CoinApp.toastLayer = new Laya.Box();
                CoinApp.toastLayer.name = "toastLayer";
                CoinApp.toastLayer.width = 750;
                CoinApp.toastLayer.height = 60;
                CoinApp.toastLayer.anchorX = 0.5;
                CoinApp.toastLayer.anchorY = 0.5;
                CoinApp.toastLayer.x = Laya.stage.width / 2;
                CoinApp.toastLayer.y = Laya.stage.height / 2;
                CoinApp.toastLayer.zOrder = 9999;
                Laya.stage.addChild(CoinApp.toastLayer);

                let bg = new Laya.Image();
                bg.skin = "coinsdk/black.png";
                bg.name = "bg";
                bg.anchorX = 0.5;
                bg.anchorY = 0.5;
                bg.width = 200;
                bg.height = 60;
                bg.x = CoinApp.toastLayer.width / 2;
                bg.y = CoinApp.toastLayer.height / 2;
                bg.alpha = 0.7;
                CoinApp.toastLayer.addChild(bg);

                let lbl = new Laya.Label();
                lbl.name = "lbl";
                lbl.fontSize = 30;
                lbl.anchorX = 0.5;
                lbl.anchorY = 0.5;
                lbl.color = "#ffffff";
                lbl.x = CoinApp.toastLayer.width / 2;
                lbl.y = CoinApp.toastLayer.height / 2;
                CoinApp.toastLayer.addChild(lbl);


            }

            let label = CoinApp.toastLayer.getChildByName("lbl");
            let bg = CoinApp.toastLayer.getChildByName("bg");

            label.text = tip;
            bg.width = label.width + 50;

            CoinApp.toastLayer.visible = true;

            let time = 500;
            CoinApp.toastLayer.y = Laya.stage.height / 2 + 100;
            let tl = Laya.TimeLine.to(CoinApp.toastLayer, { y: Laya.stage.height / 2 }, time);
            tl.play(0, false);

            setTimeout(() => {
                CoinApp.toastLayer.visible = false;
            }, 2000);
        } else {
            let fly_text = document.getElementById("fly_text");
            if (fly_text) {
                fly_text.innerHTML = tip;
                fly_text.style.display = 'block';

                // fly_text.style.top =  "700px";

                if (!CoinApp.fly_text_origin_top) {
                    CoinApp.fly_text_origin_top = parseInt(fly_text.style.top);
                }

                if (CoinApp.fly_text_timer) {
                    clearInterval(CoinApp.fly_text_timer);
                    CoinApp.fly_text_timer = null;
                }

                if (CoinApp.fly_text_delt_timer) {
                    clearInterval(CoinApp.fly_text_delt_timer);
                    CoinApp.fly_text_delt_timer = null;
                }

                fly_text.style.top = CoinApp.fly_text_origin_top + "px";
                fly_text.style.left = (window.innerWidth - fly_text.scrollWidth) / 2 + "px";
                CoinApp.fly_text_timer = setInterval(() => {
                    let temp = parseInt(fly_text.style.top);
                    temp -= 1;
                    if ((CoinApp.fly_text_origin_top - temp) >= 100) {
                        clearInterval(CoinApp.fly_text_timer);
                        CoinApp.fly_text_timer = null;

                        CoinApp.fly_text_delt_timer = setTimeout(() => {
                            fly_text.style.display = 'none';
                            clearInterval(CoinApp.fly_text_delt_timer);
                            CoinApp.fly_text_delt_timer = null;
                        }, 2000);
                    }
                    fly_text.style.top = temp + "px";
                }, 1);

            }
        }
    }

    /**
     * @description 创建竞标赛分享
     * @param score 把最好的成绩传进来
     * @param createTourSuccess 分享锦标赛成功回调
     * @param failSuccess 分享锦标赛失败回调
     *  https://developers.facebook.com/docs/games/social-and-retention/features/tournaments/instant-games
    */
    CoinApp.tournamentCreateAsync = function (score, createTourSuccess, failSuccess) {
        //如果是送审版 ***************游戏内自行完善***************
        createTourSuccess && createTourSuccess();
        createTourSuccess = null;
        return;

        // if (!FBInstant.tournament || !FBInstant.tournament.postScoreAsync || !FBInstant.tournament.createAsync) {
        //     failSuccess && failSuccess();
        //     failSuccess = null;
        //     return;
        // }

        // FBInstant.tournament.postScoreAsync(score)
        //     .then(function (e) {
        //         console.log("==同步分数成功===", JSON.stringify(e));
        //     }).catch(function (error) {
        //         console.log("==同步分数失败===", JSON.stringify(error));
        //     });

        // //检查当前是否在竞标赛中
        // FBInstant.getTournamentAsync()
        //     .then(function (tournament) {
        //         console.log('锦标赛已经存在', tournament.getContextID());
        //         console.log(tournament.getEndTime());
        //         FBInstant.tournament.shareAsync({
        //             score: score,
        //             data: { myReplayData: '...' }
        //         })
        //             .then(function () {
        //                 // continue with the game.
        //                 createTourSuccess && createTourSuccess();
        //                 createTourSuccess = null;
        //             })
        //             .catch(function (error) {
        //                 failSuccess && failSuccess();
        //                 failSuccess = null;
        //                 console.log("==晋级赛创建失败===", JSON.stringify(error));
        //             });
        //     })
        //     .catch(function (error) {

        //         console.log("==锦标赛不存在===", JSON.stringify(error));

        //         //创建竞标赛
        //         let endTime = Math.floor(new Date().getTime() / 1000) + 60 * 60 * 24 * 7;
        //         FBInstant.tournament.createAsync({
        //             initialScore: score,
        //             data: { tournamentLevel: 'hard' },
        //             config: {
        //                 title: 'Come, Have the courage to compare',
        //                 image: window.fbShareCfg.share_icon,
        //                 sortOrder: 'HIGHER_IS_BETTER',
        //                 scoreFormat: 'NUMERIC',
        //                 endTime: endTime,
        //             },
        //         }).then(function (e) {
        //             console.log("==晋级赛创建成功===", JSON.stringify(e));
        //             createTourSuccess && createTourSuccess();
        //             createTourSuccess = null;
        //         }).catch(function (error) {
        //             failSuccess && failSuccess();
        //             failSuccess = null;
        //             console.log("==晋级赛创建失败===", JSON.stringify(error));
        //         });
        //     });


    }

    /**
     * @description 提交最好成绩到排行榜
     * @param bestValue 最好成绩
     * @param success 成功回调
     * @param fail 失败回调
    */
    CoinApp.submitBestValueToRank = function (bestValue, success = null, fail = null) {
        if (!CoinApp.isSupport()) {
            return;
        }
        CoinApp.leaderboard.setScoreAsync(bestValue)
            .then(function (entry) {
                return CoinApp.leaderboard.getEntriesAsync();
            })
            .then((res) => {
                console.log("==rank data=====", res);
                success && success(res);
                success = null;
            })
            .catch(function (error) {
                console.error('Error sending score: ' + error.message);
                fail && fail(res);
                fail = null;
            });
    }

    /**
     * @description 是否支持排行榜，solo环境下不支持排行榜
    */
    CoinApp.isSupport = function () {
        CoinApp.contextID = FBInstant.context.getID();
        if (CoinApp.contextID) {
            return true;
        }
        console.log("当前环境不支持排行榜");
        return false;
    }

    /**
     * @description 获取排行榜数据
     * @param success 成功回调
     * @param fail 失败回调
    */
    CoinApp.fetchLeaderboard = function (success = null, fail = null) {
        // if (!CoinApp.isSupport()) {
        //     return;
        // }

        // // If the leaderboard name is not found in your
        // // app's configuration, the promise will reject
        // FBInstant.getLeaderboardAsync(CoinApp.LEADERBOARD_NAME + "." + CoinApp.contextID)
        //     .then(function (result) {
        //         CoinApp.leaderboard = result;
        //         let name = CoinApp.leaderboard.getName();
        //         let contextID = CoinApp.leaderboard.getContextID();
        //         console.log("==leaderboard==name===contextID==", name, contextID);
        //         return CoinApp.leaderboard.getEntriesAsync();
        //     })
        //     .then((res) => {
        //         console.log("==fetchLeaderboard=====", res);
        //         if (res && res.length) {
        //             for (let index = 0; index < res.length; index++) {
        //                 let element = res[index];
        //                 let playInfo = element.getPlayer();
        //                 console.log(element.getFormattedScore(), playInfo.getPhoto());
        //             }
        //         }
        //         success && success(res);
        //         success = null;
        //     })
        //     .catch(function (error) {
        //         console.error('Leaderboard is not found in app configuration');
        //         fail && fail(res);
        //         fail = null;
        //     });
    }

    return CoinApp;
})();

CoinApp.init = function () {
    if (!CoinApp.instance) {
        CoinApp.instance = new CoinApp();
    }
}
CoinApp.prototype.__class__ = "CoinApp";

CoinApp.init();




