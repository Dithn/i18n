# विंडोज स्टोर गाइड

विंडोज 10 से, पुराने पर बढ़िया विन32 एग्जिक्युटेबल को एक और भाई मिल गया है: वैश्विक विंडोज प्लेटफार्म | नया `.एप्पएक्स` फॉर्मेट न केवल कई सारे नये शक्तिशाली ऐपीआई जैसे कि कोरटाना या पुश नोटीफिकेशन की सुविधा देता है, पर विंडोज स्टोर के द्वारा, इंस्टालेशन और अपडेटिंग भी सरल बनाता है |

माइक्रोसॉफ्ट ने [एक औज़ार विकसित किया था जो इलेक्ट्रॉन एप्प्स को `.एप्पएक्स` पैकेजेस की तरह कम्पायल करता है](https://github.com/catalystcode/electron-windows-store), जिससे कि डेवलपर्स को नये एप्लीकेशन मॉडल में मौज़ूद कुछ अच्छी चीज़े इस्तेमाल करने की सुविधा मिलती है | यह गाइड आपको बताएगी कि इसे कैसे इस्तेमाल करना है - और इलेक्ट्रॉन एप्पएक्स पैकेज की क्या-क्या क्षमतायें और सीमायें है |

## पृष्ठभूमि और आवश्यकतायें

विंडोज 10 का "सालगिरह अपडेट" विन32 `.ईएक्सई` लाइब्रेरीज को एक वर्चुअलाइज़ेड फाइलसिस्टम और रजिस्ट्री के साथ, एक साथ लांच करने की क्षमता प्रदान करता है | दोनों ही कंपाइलेशन के दौरान एप्प और इंस्टालर को विंडोज कंटेनर के अन्दर चलाने से बनाये जाते हैं, जिससे कि विंडोज यह पता लगा सके कि इंस्टालेशन के दौरान ऑपरेटिंग सिस्टम में असल में कौन कौन से संशोधन हुए है | एक्सीक्यूटेबल को एक वर्चुअल फाइलसिस्टम और एक वर्चुअल रजिस्ट्री के साथ जोड़ने से विंडोज को 1-क्लिक इंस्टालेशन और अनइंस्टालेशन शुरू करने की सुविधा मिलती है |

इसके अलावा, ईएक्सई एप्पएक्स मॉडल के अंदर शुरू होती है - यानी कि यह वैश्विक विंडोज प्लेटफार्म में मौज़ूद बहुत सी ऐपीआई का इस्तेमाल कर सकता है | और भी ज्यादा क्षमताओं को पाने के लिए, एक इलेक्ट्रॉन एप्प एक अदृश्य युडब्ल्यूपी बैकग्राउंड टास्क के साथ जुड़ कर `ईएक्सई` के साथ लांच हो सकती है - कुछ हद तक एक वफादार साथी की तरह जो बैकग्राउंड में टास्क चला सकता है, पुश नोटिफिकेशन प्राप्त कर सकता है, या दूसरी युडब्ल्यूपी एप्लीकेशनस से संपर्क साध सकता है |

किसी भी मौजूदा इलेक्ट्रॉन एप्प को क्म्पाय्ल करने के लिए, यह सुनिश्चित कर लें की आप निम्नलिखित आवश्यकताओं को पूरा करते हों:

* विंडोज 10, सालगिरह अपडेट के साथ (2 अगस्त, 2016 को जारी)
* विंडोज 10 एसडीके, [यहाँ से डाउनलोड करें](https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk)
* न्यूनतम नोड 4 (जाँचने के लिए, `node -v` चलायें)

उसके बाद, इनस्टॉल करें `इलेक्ट्रॉन-विंडोज-स्टोर` सीएलआई:

```sh
npm install -g electron-windows-store
```

## पहला चरण: अपनी इलेक्ट्रॉन एप्लीकेशन पैकेज करें

[इलेक्ट्रॉन-पैकेजर](https://github.com/electron-userland/electron-packager) (या ऐसे ही किसी दुसरे औज़ार) से एप्लीकेशन पैकेज करें | Make sure to remove `node_modules` that you don't need in your final application, since any module you don't actually need will increase your application's size.

आउटपुट कुछ इस तरह का दिखना चाहिये:

```text
├── Ghost.exe
├── LICENSE
├── content_resources_200_percent.pak
├── content_shell.pak
├── d3dcompiler_47.dll
├── ffmpeg.dll
├── icudtl.dat
├── libEGL.dll
├── libGLESv2.dll
├── locales
│   ├── am.pak
│   ├── ar.pak
│   ├── [...]
├── natives_blob.bin
├── node.dll
├── resources
│   ├── app
│   └── atom.asar
├── v8_context_snapshot.bin
├── squirrel.exe
└── ui_resources_200_percent.pak
```

## दूसरा चरण: इलेक्ट्रॉन-विंडोज-स्टोर चलाना

एक एलिवेटेड पॉवरशैल ("एडमिनिसट्रेटर" की तरह) से, ज़रूरी पैरामीटर्स के साथ `इलेक्ट्रॉन-विंडोज-स्टोर` चलायें, दोनों इनपुट और आउटपुट डायरेक्टरी पास करें, एप्प का नाम और संस्करण, और `नोड_मोड्यूल` फ्लैट करने की पुष्टि|

```powershell
electron-windows-store `
    --input-directory C:\myelectronapp `
    --output-directory C:\output\myelectronapp `
    --flatten true `
    --package-version 1.0.0.0 `
    --package-name myelectronapp
```

एक बार चलाने के बाद, यह औज़ार काम करने लगता है: यह आपकी इलेक्ट्रॉन एप्प को एक इनपुट की तरह लेता है, और `नोड_मोड्यूल` फ्लैट करता है | फिर यह आपकी एप्लीकेशन को `एप्प.ज़िप` से आर्काइव करता है | एक इंस्टालर और विंडोज कंटेनर का इस्तेमाल कर के, यह औज़ार एक "विस्तारित" एप्पएक्स पैकेज बनाता है - और आपके आउटपुट फोल्डर में विंडोज एप्लीकेशन मेनीफेस्ट (`एप्पएक्समेनीफेस्ट.एक्सएमएल`) और साथ ही वर्चुअल फाइल सिस्टम और वर्चुअल रजिस्ट्री शामिल करता है |

एक बार विस्तारित एप्पएक्स फाइल्स निर्मित हो जाने के बाद, विंडोज एप्प पैकेजर (`मेकएप्पएक्स.ईएक्सई`) का इस्तेमाल कर के औज़ार, डिस्क पर मौज़ूद उन फाइल्स से एक एकल-फाइल एप्पएक्स का निर्माण करता है | अंत में, इस औज़ार का इस्तेमाल कर के आपके कंप्यूटर पर एक विश्वसनीय प्रमाण पत्र बनाया जा सकता है जिससे कि नये एप्पएक्स पैकेज पर हस्ताक्षर किया जा सके | हस्ताक्षरित एप्पएक्स पैकेज के साथ, सीएलआई भी स्वचालित रूप से आपकी मशीन पर पैकेज स्थापित कर सकती है ।

## तीसरा चरण: एप्पएक्स पैकेज का इस्तेमाल करना

आपके पैकेज को चलाने के लिए, उपयोगकर्ताओं को "सालगिरह अपडेट" के साथ विंडोज 10 की ज़रुरत होगी - विंडोज को अपडेट करने की विस्तृत जानकारी [यहाँ](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update) उपलब्ध है |

पारंपरिक युडब्ल्यूपी एप्प्स के विपरीत, पैकेज्ड एप्प्स को फिलहाल एक मैन्युअल जांच प्रक्रिया से गुजरना पड़ता है, जिसके के लिए आप [यहाँ](https://developer.microsoft.com/en-us/windows/projects/campaigns/desktop-bridge) पर आवेदन कर सकते है | In the meantime, all users will be able to install your package by double-clicking it, so a submission to the store might not be necessary if you're looking for an easier installation method. प्रबंधित वातावरणों में (कंपनियों में ज्यादातर), `ऐड-एप्पएक्सपैकेज` [पॉवरशैल सीएमडीलेट का इस्तेमाल उसे एक स्वचालित रूप से इनस्टॉल करने के लिए किया जा सकता है](https://technet.microsoft.com/en-us/library/hh856048.aspx) |

दूसरी महत्वपूर्ण सीमा यह है कि कम्पाइलड एप्पएक्स पैकेज में अभी भी एक विन32 एक्सीक्यूटेबल शामिल होती है - और इसलिए यह एक्सबॉक्स, होलोलेंस, या फ़ोन पर नहीं चलेगा |

## वैकल्पिक: एक बेकग्राउंड टास्क का इस्तेमाल कर युडब्ल्यूपी क्षमतायें डालें

आप अपनी इलेक्ट्रॉन एप्प को एक अदृश्य युडब्ल्यूपी बैकग्राउंड टास्क के साथ जोड़ सकते हैं जो विंडोज 10 की क्षमताओं का पूर्ण इस्तेमाल कर सके - जैसे की पुश नोटिफिकेशन, कोर्ताना एकीकरण, या लाइव टाइल्स |

यह जाँचने के लिए जो इलेक्ट्रॉन एप्प टोस्ट नोटिफिकेशन और लाइवटाइल्स के लिए बैकग्राउंड टास्क का इस्तेमाल करती है, [माइक्रोसॉफ्ट के द्वारा प्रदान सैंपल देखें](https://github.com/felixrieseberg/electron-uwp-background) |

## वैकल्पिक: कंटेनर वर्चुअलिज़ेशन का उपयोग कर कन्वर्ट करें

एप्पएक्स पैकेज को उत्पन्न करने के लिए, `इलेक्ट्रॉन -विंडोज-स्टोर` सीएलआई एक टेम्पलेट का इस्तेमाल करता है जो कि ज्यादातर इलेक्ट्रॉन एप्प्स के लिए काम करता है | हालाँकि, अगर आप एक कस्टम इंस्टालर इस्तेमाल कर रहे हैं, या फिर आपको उत्पन्न पैकेज में कोई दिक्कत आ रही है, तो आप एक विंडोज कंटेनर के साथ कंपाइलेशन का इस्तेमाल कर एक पैकेज बनाने की कोशिश कर सकते हैं - इस मोड में, सीएलआई आपकी एप्लीकेशन को एक खाली विंडोज कंटेनर में इनस्टॉल करेगा और चलायेगा ताकि यह पता चल सके कि आपकी एप्लीकेशन ऑपरेटिंग सिस्टम में वास्ताव में कौन कौन से परिवर्तन कर रही है |

सीएलआई का सबसे पहला इस्तेमाल करने से पहले, आपको "विंडोज डेस्कटॉप एप्प कनवर्टर" का सेटअप करना होगा | इसमें कुछ मिनट लग सकते हैं, पर चिंता न करें - ऐसा आपको केवल एक ही बार करना पड़ेगा | डेस्कटॉप एप्प कनवर्टर [यहाँ](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter) से डाउनलोड करें | आपको 2 फाइल्स मिलेंगी: `डेस्कटॉपएप्पकनवर्टर.ज़िप` और `बेसइमेज-14316.विम` |

1. `डेस्कटॉपएप्पकनवर्टर.ज़िप` को अनज़िप करें | एक एलिवेटेड पॉवरशैल ("एडमिनिसट्रेटर" की तरह) से, `Set-ExecutionPolicy bypass` चला कर इस बात की पुष्टि कर लें कि आपकी सिस्टम एक्ज़ेक्युशन पोलिसी हमें वो सब चलाने की इजाज़त देती है जो हम चलाना चाहते हैं |
2. उसके बाद, डेस्कटॉप एप्प कनवर्टर की इंस्टालेशन चलायें, विंडोज बेस इमेज (`बेसइमेज-14316.विम` के नाम से डाउनलोड) की लोकेशन पास करते हुए, `.\DesktopAppConverter.ps1 -Setup -BaseImage .\BaseImage-14316.wim` को कॉल कर के |
3. अगर उपर दी गयी कमांड चलाने से आपको रिबूट करने का सन्देश मिलता है, तो कृपया अपना कंप्यूटर रीस्टार्ट करें और ऊपर दी गयी कमांड दोबारा चलायें |

एक बार जब इंस्टालेशन कामयाब हो जाये, तो फिर आप अपनी इलेक्ट्रॉन एप्प को दोबारा से कम्पाईल करना शुरू कर सकते हैं |