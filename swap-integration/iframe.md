---
description: >-
    This guide walks you through the steps to embed the Mosaic iframe in your website in a few minutes.
---

# Mosaic Iframe
Mosaic can be used within other sites as an iframe. An iframe shows an exact version of the Mosaic frontend site and can have custom prefilled settings.


One benefit of an iframe integration is that the your site will automatically keep up with any improvements/additions to the site. After the initital integration is setup no further work is needed to pull in updates as the exchange site is updated over time.


## Url format: 
```js
https://app.mosaic.ag/swap/tokenA-tokenB?param1=value1&param2=value2&...
```

- tokenA, tokenB: fa address (recommended) or coinType, ex: `0xe161897670a0ee5a0e3c79c3b894a0c46e4ba54c6d2ca32e285ab4b01eb74b66` or `0x275f508689de8756169d1ee02d889c777de1cebda3a7bbcce63ba8a27c563c6f::tokens::USDT`

### Parameters


| **Params** | **Required** | **Description**         | **Default Value** |
|------------|--------------|-------------------------|--------------------|
| apiKey     | Yes          |  Register to get a new one, <a href='https://docs.mosaic.ag/swap-integration/integration-partners'>detail here</a>    |          |
| amount     | No           | Default input amount   | 1           |
| isFeeIn     | No          | Charge fee by token in, default is token out   | false           |
| feeInBps     | No          | Fee amount to be collected. (i.e. feeAmount = 10 then fee = 0.1%)   |            |
| feeReceiver     | No          | Fee receiver address   |            |


### Adding the iFrame to your site

Example code:

```js
    <iframe
      style={{ margin: 'auto' }}
      width="500px"
      height="800px"
      src="https://app.mosaic.ag/swap/0xa-0xe161897670a0ee5a0e3c79c3b894a0c46e4ba54c6d2ca32e285ab4b01eb74b66?amount=2&isFeeIn=true&feeInBps=30&feeReceiver=0xb9309aedd0dca69145c51003e32d097b9f8795d0045e26d9bc924dd4c199ec92&apiKey=key"
    />
```




### Feature request & Report issue

- Feel free to contact us to request new config/report bug if you think it might be helpful.
- Please reach out to us via Discord or email us directly at **huy@mosaic.ag** for assistance and further details.