From https://docs.google.com/spreadsheets/d/1p1BLCfuYoKHGRnBW-Aj_lOLzjx0jCIB0Uw1udUv9Ua0/

Also check out https://exceljet.net/how-to-use-formula-criteria

## Make variables
For sharing:
* FACEBOOKSHARE
* TWITTERSHARE
* LINKEDINSHARE
* PINTRESTPIN
* GOOGLEPLUSSHARE

For bitly
1. Log in to Bit.ly | https://bitly.com/a/sign_in
1. No Bit.Ly Account? Sign Up here --->	https://bitly.com/a/sign_up
2. Create a Generic Access Token --->	https://bitly.com/a/oauth_apps
3. Paste Your Generic Access Token --->	

* ACCESSTOKENBIT: Should be the cell where you put your bit.ly access token.
```
https://www.facebook.com/sharer/sharer.php?u=
https://twitter.com/intent/tweet?text=Edit%20this%20text%20as%20per%20your%20need&url=
https://www.linkedin.com/shareArticle?mini=true&url=
http://pinterest.com/pin/create/button/?url=
https://plus.google.com/share?url=
```
Put the above in a cell each and name them with `Data`->`Named Ranges`.

## Check that no fields are blank
`D,E,F` are compulsory. `G,H,I` are optional.
```
=IF(
    OR(
        LEN(
            $D11
        ),
        LEN(
            $E11
        ),
        LEN(
            $F11
        ),
        LEN(
            $G11
        ),
        LEN(
            $H11
        ),
        LEN(
            $I11
        )
    ),
    IF(
        LEN(
            $D11
        ),
        IF(
            LEN(
                $E11
            ),
            IF(
                LEN(
                    $F11
                ),
                CONCATENATE(
                    $I11,
                    CONCATENATE(
                        IF(
                            ISERROR(
                                FIND(
                                    "?",
                                    $I11,
                                    1
                                )
                            ),
                            "?",
                            "&"
                        ),
                        "utm_medium="
                    ),
                    $D11,
                    IF(
                        LEN(
                            $E11
                        ),
                        CONCATENATE(
                            "&utm_source=",
                            (                                
                                SUBSTITUTE(
                                    $E11,
                                    " ",
                                    "%20"
                                ) )
                        ),
                        ""
                    ),
                    "&utm_campaign=",
                    SUBSTITUTE(
                        $F11,
                        " ",
                        "%20"
                    ),
                    IF(
                        LEN(
                            $H11
                        ),
                        CONCATENATE(
                            "&utm_term=",
                            ( 
                                SUBSTITUTE(
                                    H11,
                                    " ",
                                    "%20"
                                ) )
                        ),
                        ""
                    ),
                    IF(
                        LEN(
                            $G11
                        ),
                        CONCATENATE(
                            "&utm_content=",
                            (
                                SUBSTITUTE(
                                    $G11,
                                    " ",
                                    "%20"
                                ) )
                        ),
                        ""
                    )
                ),
                "Campaign Name is Mandatory."
            ),
            "Source is Mandatory."
        ),
        "Medium is Mandatory."
    ),
    ""
)
```


## Get Bit.ly URL using access token
If `J` is a valid URL, get bit.ly shortened link. 
```
=IF(
    ISURL(
        J12
    ),
    IMPORTDATA(
        CONCATENATE(
            "https://api-ssl.bitly.com/v3/shorten?format=txt&access_token=",
            ACCESSTOKENBIT,
            "&longUrl=",
            SUBSTITUTE(
                SUBSTITUTE(
                    SUBSTITUTE(
                        SUBSTITUTE(
                            SUBSTITUTE(
                                J12,
                                "%",
                                "%25"
                            ),
                            "=",
                            "%3D"
                        ),
                        "&",
                        "%26"
                    ),
                    "?",
                    "%3F"
                ),
                "#",
                "%23"
            )
        )
    ),
    ""
)
```

### Get click rate of bit.ly
```
=IF(
    ISURL(
        K9
    ),
     IMPORTDATA(
        CONCATENATE(
            "https://api-ssl.bitly.com/v3/link/clicks?format=txt&unit=day&units=-1&rollup=true&access_token=",
              ACCESSTOKENBIT,
            "&link=",
            K9
        )
    ),
    ""
)
```

## Get tinyurl
```
=IF(
    ISURL(
        J11
    ),
    importData(
        concatenate(
            "http://tinyurl.com/api-create.php?url=",
            J11
        )
    ),
    ""
)
```

## Sharing
### Facebook
```
=IF(
    ISURL(
        J10
    ),
    CONCAT(
        FACEBOOKSHARE,
        J10
    ),
    ""
)
```

### Twitter
```
=IF(
    ISURL(
        J9
    ),
    CONCAT(
        TWITTERSHARE,
        J9
    ),
    ""
)
```

### LinkedIN
```
=IF(
    ISURL(
        J9
    ),
    CONCAT(
        LINKEDINSHARE,
        J9
    ),
    ""
)
```

### Pintrest
```
=IF(
    ISURL(
        J9
    ),
    CONCAT(
        PINTERESTPIN,
        J9
    ),
    ""
)
```