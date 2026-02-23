# regex
My mad regex addiction

## validate dates
Obs.: DO NOT use this in production or anywhiri to validate dates. Simply try to instantiate a date object in your language of choice and see if it succeeds. If it does, it's valid. If it doesn't, it's not.
To test the `validDate` regex, go [here](https://regex101.com/r/KqYEUV/1)
```
^(?:(?P<dia0_19dez>[0-1])|(?P<dia20_29dez>2)|(?P<dia30_31dez>3))(?(dia0_19dez)[0-9])(?(dia20_29dez)(?:(?P<dia0_8uni>[0-8])|(?P<dia9uni>9)))(?(dia30_31dez)(?:(?P<dia30uni>0)|(?P<dia31uni>1)))\/(?:(?P<mes_31_dias>01|03|05|07|08|10|12)|(?P<mes_30_dias>04|06|09|11)|(?P<fev>02))(?(dia30_31dez)(?(fev)(?!)))(?(mes_30_dias)(?(dia31uni)(?!)))/(?:(?P<anomilcenmult4>04|08|12|16|20|24|28|32|36|40|44|48|52|56|60|64|68|72|76|80|84|88|92|96)|\d\d)(?:(?P<anodezunimult4>04|08|12|16|20|24|28|32|36|40|44|48|52|56|60|64|68|72|76|80|84|88|92|96)|(?P<anodezuni00>00)|\d\d)(?(fev)(?(dia20_29dez)(?(dia9uni)(?(anodezuni00)(?(anomilcenmult4)|(?!))|(?(anodezunimult4)(?(anomilcenmult4)|(?!))|(?!))))))
```

## validate email address
Obs.: DO NOT use this in production or anywhere to validate email addresses. First of all, no provider is **ACTUALLY** obliged to follow this spec. Second of all, just send an email message with a link and if the link is clicked, you know it's valid.
To test it, go [here](https://regex101.com/r/aick0I/1)
```
(?(DEFINE)
	(?'vchar'
		[\x21-\x7e]
	)
	(?'wsp'
		[\x20\x09]
	)
	(?'obsnowsctl'
		[\x01-\x08\x0b\x0c\x0d-\x1f\x7f]
	)
	(?'obsctext'
		(?P>obsnowsctl)
	)
	(?'obsqtext'
		(?P>obsnowsctl)
	)
	(?'obsutext'
		(?:\x00|(?P>obsnowsctl)|(?P>vchar))
	)
	(?'obsqp'
		\\(?:\x00|(?P>obsnowsctl)|\x0A|\x0d)
	)
	(?'dtext'
		(?:[\x21-\x5a\x5e-\x7e]|(?P>obsdtext))
	)
	(?'quotedpair'
		(?:\\(?:(?P>vchar)|(?P>wsp))|(?P>obsqp))
	)
	(?'obsfws'
		(?P>wsp)+(?:\x0d\x0a(?P>wsp))*
	)
	(?'fws'
		(?:(?:(?P>wsp)(?:\x0d\x0a))?(?P>wsp)+)|(?P>obsfws)
	)
	(?'obsdtext'
		(?P>obsnowsctl)|(?P>quotedpair)
	)
	(?'ctext'
		(?:[\x21-\x27\x2a-\x5b\x5d-\x7e])|(?P>obsctext)
	)
	(?'comment'
		\((?:(?P>fws)?(?P>ccontent))*(?P>fws)?\)
	)
	(?'ccontent'
		(?P>ctext)|(?P>quotedpair)|(?P>comment)
	)
	(?'cfws'
		(?:(?:(?P>fws)?(?P>comment))+(?P>fws)?)|(?P>fws)
	)
	(?'atext'
		[a-zA-Z0-9!#$%&'*+\-\/=?\^_`{|}~]
	)
	(?'word'
		(?P>atom)|(?P>quotedstring)
	)
	(?'obslocalpart'
		(?P>word)(?:\.(?P>word))*
	)
	(?'obsdomain'
		(?P>atom)(?:\.(?P>atom))*
	)
	(?'atom'
		(?P>cfws)?(?P>atext)+(?P>cfws)?
	)
	(?'qtext'
		(?:[\x21\x23-\x5b\x5d-\x7e]|(?P>obsqtext))
	)
	(?'qcontent'
		(?P>qtext)|(?P>quotedpair)
	)
	(?'quotedstring'
		(?P>cfws)?(?:(?P>fws)?(?P>qcontent))*(?P>fws)?\"(?P>cfws)?
	)
	(?'dotatomtext'
		(?P>atext)+(?:\.(?P>atext)+)*
	)
	(?'dotatom'
		(?P>cfws)(?P>dotatomtext)(?P>cfws)
	)
	(?'localpart'
		(?P>dotatom)|(?P>quotedstring)|(?P>obslocalpart)
	)
	(?'domainliteral'
		(?P>cfws)?\[(?:(?P>fws)?(?P>dtext))*(?P>fws)?\](?P>cfws)
	)
	(?'domain'
		(?P>dotatom)|(?P>domainliteral)|(?P>obsdomain)
	)
	(?'addrspec'
		(?P>localpart)@(?P>domain)
	)
)
^(?P>addrspec)$
```
