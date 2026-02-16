# Test Cards

Test cards - Without 3DS

| Number           | Description      |
| ---------------- | ---------------- |
| 5163613613613613 | Mastercard Debit |
| 5555555555554444 | Mastercard       |
| 5454545454545454 | Mastercard       |
| 2221000000000009 | Mastercard       |
| 4462030000000000 | Visa Debit       |
| 4444333322221111 | Visa             |
| 4911830000000    | Visa             |
| 4917610000000000 | Visa             |
| 343434343434343  | Amex             |
| 6011000400000000 | Discover         |
| 36700102000000   | Diners           |

Test cards - With 3DS

| Number           | Description      |
| ---------------- | ---------------- |
| 5413330033003303 | Mastercard Debit |
| 5169527513596963 | Mastercard       |
| 4462030000000000 | Visa Debit       |
| 4983305199046950 | Visa             |
| 6011000400000000 | Discover         |

Test cards - With Data-Only 3DS

| Number           | Description |
| ---------------- | ----------- |
| 5454545454545454 | Mastercard  |
| 4975303994654672 | Visa        |
| 6011926557021045 | Discover    |

In order to simulate acquirer authorization error codes and validations, please refer to the table:

| Name       | Result                                        |
| ---------- | --------------------------------------------- |
| AUTHORISED | HTTP 200                                      |
| REFUSED    | HTTP 400                                      |
| ERROR      | HTTP 400                                      |
| REFUSED4   | HTTP 400, Hold card                           |
| REFUSED5   | HTTP 400, General refuse                      |
| REFUSED13  | HTTP 400, Invalid amount                      |
| REFUSED33  | HTTP 400, Card Expired                        |
| REFUSED34  | HTTP 400, Suspicious activity, possible fraud |
| REFUSED41  | HTTP 400, Lost Card                           |
| REFUSED43  | HTTP 400, Stolen Card                         |
| REFUSED55  | HTTP 400, Invalid CVV                         |
| REFUSED76  | HTTP 400, Card blocked                        |
| REFUSED85  | HTTP 400, Rejected by the card issuer         |

CVV and AVS test values for Mastercard/Visa/Diners/Discover

| Name      | Result                                       |
| --------- | -------------------------------------------- |
| < empty > | B - CVV/CVC not supplied by shopper/merchant |
| 111       | C - CVV/CVC not checked                      |
| 222       | C - CVV/CVC not checked                      |
| 333       | C - CVV/CVC not checked                      |
| 444       | D - CVV/CVC not matched                      |
| 555       | A - CVV/CVC matched                          |

CVV and AVS test values for Amex

| Name      | Result                                       |
| --------- | -------------------------------------------- |
| < empty > | B - CVV/CVC not supplied by shopper/merchant |
| 1111      | C - CVV/CVC not checked                      |
| 2222      | C - CVV/CVC not checked                      |
| 3333      | C - CVV/CVC not checked                      |
| 4444      | D - CVV/CVC not matched                      |
| 5555      | C - CVV/CVC not checked                      |
| 6666      | A - CVV/CVC matched                          |

AVS (Address verification system), that matches cardholder billing information

| Name          | Result                    | Description                                           |
| ------------- | ------------------------- | ----------------------------------------------------- |
| AAAA or AAAAA | APPROVED                  | Postcode and address matched                          |
| BBBB or BBBBB | PARTIAL APPROVED          | Postcode matched; address not checked                 |
| CCCC or CCCCC | PARTIAL APPROVED          | Postcode matched; address not matched                 |
| DDDD or DDDDD | PARTIAL APPROVED          | Address matched; postcode not checked                 |
| EEEE or EEEEE | NOT SENT TO ACQUIRER      | Postcode and address not checked                      |
| FFFF or FFFFF | PARTIAL APPROVED          | Address matched; postcode not matched                 |
| GGGG or GGGGG | PARTIAL APPROVED          | Postcode not checked; address not matched             |
| HHHH or HHHHH | NOT SUPPLIED BY SHOPPER   | Postcode and address not supplied by shopper/merchant |
| IIII or IIIII | PARTIAL APPROVED          | Address not checked; postcode not matched             |
| JJJJ or JJJJJ | FAILED                    | Postcode and address not matched                      |
| < empty >     | NOT SUPPLIED BY SHOPPER   | Postcode and address not supplied by shopper/merchant |
| KKKK or KKKKK | NO RESPONSE FROM ACQUIRER | Postcode and address not checked                      |
| LLLL or LLLLL | NOT CHECKED BY ACQUIRER   | Postcode and address not checked                      |
| MMMM or MMMMM | UNKNOWN                   | Postcode and address not checked                      |
