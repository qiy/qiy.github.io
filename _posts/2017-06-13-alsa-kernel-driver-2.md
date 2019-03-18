---
layout: post
title:  "音频kernel部分[架构二]"
categories: ALSA
tags:  alsa linux kernel audio
author: phone
---

* content
{:toc}
## 一：alsa kernel部分主要提供两个东西 ##
01:controlc* 一般只有controlc0
02:pcmc*d*p/c  23个cpm，c*  card;d* device;p/c p：play back（播放）c：capture（录音）；


*WELCOME TO READ ME*


```
xxx:/ # cd dev/snd/
xxx:/dev/snd # ls -l
total 0
crw-rw---- 1 system audio  14,  12 2016-12-04 01:03 adsp
crw-rw---- 1 system audio  14,   4 2016-12-04 01:03 audio
crw-rw---- 1 system audio 116,  36 2016-12-04 01:03 comprC0D23
crw-rw---- 1 system audio 116,   2 2016-12-04 01:03 controlC0
crw-rw---- 1 system audio  14,   3 2016-12-04 01:03 dsp
crw-rw---- 1 system audio  14,   0 2016-12-04 01:03 mixer
crw-rw---- 1 system audio 116,   3 2016-12-04 01:03 pcmC0D0p
crw-rw---- 1 system audio 116,  19 2016-12-04 01:03 pcmC0D10p
crw-rw---- 1 system audio 116,  20 2016-12-04 01:03 pcmC0D11p
crw-rw---- 1 system audio 116,  21 2016-12-04 01:03 pcmC0D12c
crw-rw---- 1 system audio 116,  22 2016-12-04 01:03 pcmC0D13c
crw-rw---- 1 system audio 116,  23 2016-12-04 01:03 pcmC0D14p
crw-rw---- 1 system audio 116,  24 2016-12-04 01:03 pcmC0D15c
crw-rw---- 1 system audio 116,  25 2016-12-04 01:03 pcmC0D16c
crw-rw---- 1 system audio 116,  27 2016-12-04 01:03 pcmC0D17c
crw-rw---- 1 system audio 116,  26 2016-12-04 01:03 pcmC0D17p
crw-rw---- 1 system audio 116,  28 2016-12-04 01:03 pcmC0D18p
crw-rw---- 1 system audio 116,  29 2016-12-04 01:03 pcmC0D19p
crw-rw---- 1 system audio 116,   4 2016-12-04 01:03 pcmC0D1c
crw-rw---- 1 system audio 116,  30 2016-12-04 01:03 pcmC0D20p
crw-rw---- 1 system audio 116,  31 2016-12-04 01:03 pcmC0D21p
crw-rw---- 1 system audio 116,  34 2016-12-04 01:03 pcmC0D22c
crw-rw---- 1 system audio 116,  35 2016-12-04 01:03 pcmC0D24p
crw-rw---- 1 system audio 116,   6 2016-12-04 01:03 pcmC0D2c
crw-rw---- 1 system audio 116,   5 2016-12-04 01:03 pcmC0D2p
crw-rw---- 1 system audio 116,   8 2016-12-04 01:03 pcmC0D3c
crw-rw---- 1 system audio 116,   7 2016-12-04 01:03 pcmC0D3p
crw-rw---- 1 system audio 116,  10 2016-12-04 01:03 pcmC0D4c
crw-rw---- 1 system audio 116,   9 2016-12-04 01:03 pcmC0D4p
crw-rw---- 1 system audio 116,  12 2016-12-04 01:03 pcmC0D5c
crw-rw---- 1 system audio 116,  11 2016-12-04 01:03 pcmC0D5p
crw-rw---- 1 system audio 116,  14 2016-12-04 01:03 pcmC0D6c
crw-rw---- 1 system audio 116,  13 2016-12-04 01:03 pcmC0D6p
crw-rw---- 1 system audio 116,  16 2016-12-04 01:03 pcmC0D7c
crw-rw---- 1 system audio 116,  15 2016-12-04 01:03 pcmC0D7p
crw-rw---- 1 system audio 116,  17 2016-12-04 01:03 pcmC0D8p
crw-rw---- 1 system audio 116,  18 2016-12-04 01:03 pcmC0D9c
crw-rw---- 1 system audio 116,   1 2016-12-04 01:03 seq
crw-rw---- 1 system audio  14,   1 2016-12-04 01:03 sequencer
crw-rw---- 1 system audio  14,   8 2016-12-04 01:03 sequencer2
crw-rw---- 1 system audio 116,  33 2016-12-04 01:03 timer
xxx:/dev/snd # 
```

---

## 二：kernel部分代码组成 ##

![](http://i.imgur.com/dY6wJxP.png)

**五部分：**

> 01:platform（pcm）: 一般是指某一个SoC平台，比如MT6582, MT6595, MT6752等等，与音频相关的通常包含该SoC中的Clock、FAE、I2S、DMA等等
> 
> 02:platform_dai(cpu_dai):连接platform和machine；
> 
> 03:codec:字面上的意思就是编解码器，Codec里面包含了I2S接口、DAC、ADC、Mixer、PA（功放），通常包含多种输入（Mic、Line-in、I2S、PCM）和多个输出（耳机、喇叭、听筒，Line-out），Codec和Platform一样，是可重用的部件;
> 
> 04:codec_dai:连接codec和machine；
> 
> 05:machine：连接platform，coedec；

**（I2S   ：数字音频接口，用于CPU和Codec之间的数字音频流raw data的传输。每当有playback或record操作时，snd_soc_dai_ops.prepare()会被调用，启动I2S总线。）**

---


## 三：对pcm，cpu_dai，codec和codec_dai注册流程 ##

> 01：pcm：（录音为例）soc/mediatek/mt_soc_audio_v3/mt_soc_pcm_capture.c
> 
> 02：cpu_dai：soc/mediatek/mt_soc_audio_v3/mt_soc_dai_stub.c
> 
> 03：codec和codec_dai：soc/mediatek/mt_soc_audio_v3/mt_soc_codec_63xx.c

**对于probe之前为传统linux kernel写法三者类同；**
![](http://i.imgur.com/VI4xt38.png)
--

对于probe往下三个分别为:

1. pcm:
![](http://i.imgur.com/7d8PRYF.png)
2. cpu_dai:
![](http://i.imgur.com/vJZ8Uis.png)
3. codec和codec_dai:
![](http://i.imgur.com/Qfam1n9.png)
颜色不同为函数内函数；

---

## 四：machine（注意下面①②③） ##

①绑定

②向系统注册control，pcm；

③调用pcm，codec中的probe;

打开kernel-3.18/sound$ vi soc/soc-core.c

soc_probe函数

```js
static int soc_probe(struct platform_device *pdev)
{
    struct snd_soc_card *card = platform_get_drvdata(pdev);//（☆②）

    /*   
     * no card, so machine driver should be registering card
     * we should not be here in that case so ret error
     */
    if (!card)
        return -EINVAL;

    dev_warn(&pdev->dev,
         "ASoC: machine %s should use snd_soc_register_card()\n",
         card->name);

    /* Bodge while we unpick instantiation */
    card->dev = &pdev->dev;

    return snd_soc_register_card(card); //函数作用？card从何来（☆①）
}
```



在：soc/mediatek/mt_soc_audio_v3/mt_soc_machine.c中：
**
①绑定（向下）
![](http://i.imgur.com/m4g9pGc.png)
绑定结束；**

** ②snd_card_new注册controlC*函数向系统注册（向上函数）

③soc_probe_link_dais--soc_new_pcm注册pcmC*D*c/p

④调用platform，codec里面的probe函数**

![](http://i.imgur.com/2NjLMyi.png)
;
![](http://i.imgur.com/j93ytX3.png)
其中：
sound/core/pcm.c

```js
snd_pcm_dev_register
{
,,,
      switch (cidx) {
        case SNDRV_PCM_STREAM_PLAYBACK:
            sprintf(str, "pcmC%iD%ip", pcm->card->number, pcm->device);
            devtype = SNDRV_DEVICE_TYPE_PCM_PLAYBACK;
            break;
        case SNDRV_PCM_STREAM_CAPTURE:
            sprintf(str, "pcmC%iD%ic", pcm->card->number, pcm->device);
            devtype = SNDRV_DEVICE_TYPE_PCM_CAPTURE;
            break;
        }

,,,

}
,,,
        /* register pcm */
        err = snd_register_device_for_dev(devtype, pcm->card,
                        ¦ pcm->device,
                        ¦ &snd_pcm_f_ops[cidx],
                        ¦ pcm, str, dev);
```
有：
![](http://i.imgur.com/qMxjiyf.png)
对于substream：

在soc/soc-pcm.c中

```js
if (rtd->dai_link->no_pcm) {
        if (playback)
            pcm->streams[SNDRV_PCM_STREAM_PLAYBACK].substream->private_data = rtd; 
        if (capture)
            pcm->streams[SNDRV_PCM_STREAM_CAPTURE].substream->private_data = rtd; 
        goto out; 
    }

```


---


## 五 codec，pcm中 probe函数 ##

①对于mt6331_snd_controls:

soc/mediatek/mt_soc_audio_v3/mt_soc_codec_63xx.c

中：
```js
static int mt6331_codec_probe(struct snd_soc_codec *codec) 

{

,,,

 snd_soc_add_codec_controls(codec, mt6331_snd_controls, ARRAY_SIZE(mt6331_snd_controls)); 

,,,

}
```
；
```js
static const struct snd_kcontrol_new mt6331_snd_controls[] = {
    SOC_ENUM_EXT("Audio_Amp_R_Switch", Audio_DL_Enum[0], Audio_AmpR_Get, Audio_AmpR_Set),
    SOC_ENUM_EXT("Audio_Amp_L_Switch", Audio_DL_Enum[1], Audio_AmpL_Get, Audio_AmpL_Set),
    SOC_ENUM_EXT("Voice_Amp_Switch", Audio_DL_Enum[2], Voice_Amp_Get, Voice_Amp_Set),
    SOC_ENUM_EXT("Speaker_Amp_Switch", Audio_DL_Enum[3], Speaker_Amp_Get, Speaker_Amp_Set),
    SOC_ENUM_EXT("Headset_Speaker_Amp_Switch", Audio_DL_Enum[4], Headset_Speaker_Amp_Get,
          ¦  Headset_Speaker_Amp_Set),
    SOC_ENUM_EXT("Headset_PGAL_GAIN", Audio_DL_Enum[5], Headset_PGAL_Get, Headset_PGAL_Set),
    SOC_ENUM_EXT("Headset_PGAR_GAIN", Audio_DL_Enum[6], Headset_PGAR_Get, Headset_PGAR_Set),
    SOC_ENUM_EXT("Handset_PGA_GAIN", Audio_DL_Enum[7], Handset_PGA_Get, Handset_PGA_Set),
    SOC_ENUM_EXT("Lineout_PGAR_GAIN", Audio_DL_Enum[8], Lineout_PGAR_Get, Lineout_PGAR_Set),
    SOC_ENUM_EXT("Lineout_PGAL_GAIN", Audio_DL_Enum[9], Lineout_PGAL_Get, Lineout_PGAL_Set),
    SOC_ENUM_EXT("AUD_CLK_BUF_Switch", Audio_DL_Enum[10], Aud_Clk_Buf_Get, Aud_Clk_Buf_Set),
    SOC_ENUM_EXT("Ext_Speaker_Amp_Switch", Audio_DL_Enum[11], Ext_Speaker_Amp_Get,
          ¦  Ext_Speaker_Amp_Set),
    SOC_ENUM_EXT("Receiver_Speaker_Switch", Audio_DL_Enum[11], Receiver_Speaker_Switch_Get,
          ¦  Receiver_Speaker_Switch_Set),
    SOC_SINGLE_EXT("Audio HP Impedance", SND_SOC_NOPM, 0, 512, 0, Audio_Hp_Impedance_Get,
          ¦   ¦Audio_Hp_Impedance_Set),
};
```
；

```js
struct snd_kcontrol_new {
    snd_ctl_elem_iface_t iface;    /* control的类型*/
    unsigned int device;        /* device/client number */
    unsigned int subdevice;        /* subdevice (substream) number */
    const unsigned char *name;    /* control的名字*/
    unsigned int index;        /*如果声卡中有不止一个codec，每个codec中有相同名字的control，这时我们可以通过index来区分这些controls*/
    unsigned int access;        /* access rights 可以通过|或增加权限*/
    unsigned int count;        /* count of same elements */
    snd_kcontrol_info_t *info;
    snd_kcontrol_get_t *get;
    snd_kcontrol_put_t *put;
    union {
        snd_kcontrol_tlv_rw_t *c;
        const unsigned int *p;
    } tlv;
```
对于snd_soc_add_codec_controls函数(以下代码与第四项无用一样函数)：

 snd_soc_add_codec_controls---snd_soc_add_component_controls---snd_soc_add_controls---snd_ctl_add---snd_soc_cnew---snd_ctl_new1---snd_ctl_new：

其中：core/control.c

```js
struct snd_kcontrol *snd_ctl_new1(const struct snd_kcontrol_new *ncontrol,
                ¦ void *private_data)
{
    struct snd_kcontrol kctl;
    unsigned int access;
    
    if (snd_BUG_ON(!ncontrol || !ncontrol->info))
        return NULL;
    memset(&kctl, 0, sizeof(kctl));
    kctl.id.iface = ncontrol->iface;
    kctl.id.device = ncontrol->device;
    kctl.id.subdevice = ncontrol->subdevice;
    if (ncontrol->name) {
        strlcpy(kctl.id.name, ncontrol->name, sizeof(kctl.id.name));
        if (strcmp(ncontrol->name, kctl.id.name) != 0)
            pr_warn("ALSA: Control name '%s' truncated to '%s'\n",
                ncontrol->name, kctl.id.name);
    }
    kctl.id.index = ncontrol->index;
    kctl.count = ncontrol->count ? ncontrol->count : 1;
    access = ncontrol->access == 0 ? SNDRV_CTL_ELEM_ACCESS_READWRITE :
        ¦(ncontrol->access & (SNDRV_CTL_ELEM_ACCESS_READWRITE|
                ¦   ¦ SNDRV_CTL_ELEM_ACCESS_VOLATILE|
                ¦   ¦ SNDRV_CTL_ELEM_ACCESS_INACTIVE|
                ¦   ¦ SNDRV_CTL_ELEM_ACCESS_TLV_READWRITE|
                ¦   ¦ SNDRV_CTL_ELEM_ACCESS_TLV_COMMAND|
                ¦   ¦ SNDRV_CTL_ELEM_ACCESS_TLV_CALLBACK));
    kctl.info = ncontrol->info;
    kctl.get = ncontrol->get;
    kctl.put = ncontrol->put;
    kctl.tlv.p = ncontrol->tlv.p;
    kctl.private_value = ncontrol->private_value;
    kctl.private_data = private_data;
    return snd_ctl_new(&kctl, access);
}   

EXPORT_SYMBOL(snd_ctl_new1);
```

将put，get，等挂载到标准linux snd_kcontrol list;达到系统调用； 

 
 ②对于soc/mediatek/mt_soc_audio_v3/mt_soc_pcm_capture.c中mtk_afe_capture_probe:

```js
static int mtk_afe_capture_probe(struct snd_soc_platform *platform)
{
    pr_warn("mtk_afe_capture_probe\n");
    mCapturePrepare = false;
    AudDrv_Allocate_mem_Buffer(platform->dev, Soc_Aud_Digital_Block_MEM_VUL, UL1_MAX_BUFFER_SIZE);
    Capture_dma_buf =  Get_Mem_Buffer(Soc_Aud_Digital_Block_MEM_VUL);
    mAudioDigitalI2S =  kzalloc(sizeof(AudioDigtalI2S), GFP_KERNEL);
    return 0;
}

```
.probe = mtk_afe_capture_probe(snd_soc_platform *)
1. AudDrv_allocate_mem_buffer,用dma_alloc_coherent分配DMA内存.
2.Get_Mem_Buffer() 返回snd_dma_buffer*指针
3.kzalloc mAudioDigitalI2S

---

##  六 调用流程： ##

01：sound/core/sound.c 
```js
static const struct file_operations snd_fops =
{
    .owner =    THIS_MODULE,
    .open =     snd_open, //①
    .llseek =   noop_llseek,
};
```
；
```js
static int snd_open(struct inode *inode, struct file *file) //①
{
    unsigned int minor = iminor(inode);
    struct snd_minor *mptr = NULL;
    const struct file_operations *new_fops;
    int err = 0;

    if (minor >= ARRAY_SIZE(snd_minors))
        return -ENODEV;
    mutex_lock(&sound_mutex);
    mptr = snd_minors[minor];
    if (mptr == NULL) {
        mptr = autoload_device(minor);
        if (!mptr) {
            mutex_unlock(&sound_mutex);
            return -ENODEV;
        }   
    }   
    new_fops = fops_get(mptr->f_ops);
    mutex_unlock(&sound_mutex);
    if (!new_fops)
        return -ENODEV;
    replace_fops(file, new_fops);

    if (file->f_op->open)
        err = file->f_op->open(inode, file); //②
    return err;
}

```
sound/core/pcm_native.c :snd_pcm_playback_open②

sound/core/pcm_native.c :sound/core/pcm_native.c ③

sound/core/pcm_native.c :  snd_pcm_open_file ④

sound/core/pcm_native.c :  snd_pcm_open_substream ⑤


controlc设备雷同
![](http://i.imgur.com/uyWmkhj.png)
；
![](http://i.imgur.com/hkTOUQk.png)

；
