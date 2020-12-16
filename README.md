# libreelec_emuelec_h5

這邊會記錄我在整合時發生的一些狀況，而且這是我的理解，可能理解也有錯誤，請小心服用。</P>

1:首先要了解到 emuelec 本就就只有支援(或稱維護)amlogic的部份。很多程式及script基上都是根據amlogic特有的kernel裝置設備所處理。很多的功能由其在解析度上更是如此。
所以基本上在emuelec目錄下的scripts都要改一下。有少部份是直接hard-code在ES上，不過ES那部份我還沒去了解</P>
2:gcc的版本。不同版的gcc會造成編譯的的錯誤。所以移植時最好要選定是依emuele為主還是libreelec為主。個人建議依emuelec為主，至少在相關的模擬器上不會出現問題。不然可能要自已建一些patch。</P>
3:u-boot 因為我是依 libreelec為主把emuelec移過來。在 extlinux 上需加上 video=HDMI-A-1:1280x720@60。不然 retroarch 會抓不到營幕。</P>
4:lima+mesa 與 mali 官方驅動。除了mali在模擬器的效能會高於 lima，但在kodi播放影片上上卻是lima作的比較好 。且lima的3D效能實在不怎麼樣，我估計h5大該只能玩ps1以下。
psp、saturn、n64、dc 大概都不行。目前慢慢的 fbdev都會被 kmsdrm所取代，如ordroid go 上的就是使用 kmsdrm+gbm 。我曾試過用用 mali r9p0 + mali gbm。不過畫面直接破圖且慢。</P>
5:kmsdrm在es上的影響。基本上 ES 上沒有任何一點kms的程式在上頭，它完全是由sdl2上的kms去作處理。因為kms的裝置會有lock的問題所以在 ES launch retroarch 之前的 window->deinit 
跟結束模擬器返回 es 的 window->init 一定要作。retropie 的 es 跟 batocera-emulationstation 的作法有小差異。在不改程式下 retropie的es相容性會高於 batocera-emulationstation。</P>
6:retroarch 要設定 menu_enable_widgets=false。不然模擬器會有聲音，但沒畫面。估計是retroarch的問題。

結尾</P>
上述的地雷我都踩了一遍才成功用 lima+mesa+mainline kernel 整合成功。雖然效能不如mali官方的驅動3D的效能連h3都不如。但至少能玩2d的模擬器。


