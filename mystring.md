*#include* "Includes.h"





*//************************************************

unsigned char ValueToChar(unsigned char *value*)

{

  *if* (*value* <= 9)

  {

​    *return* '0' + *value*;

  }

  *else*

  {

​    *return* 'A' + *value* - 10;

  }

}





*//*******************************************

unsigned char CharToValue(unsigned char *ucChar*)

{

  *if* ((*ucChar* >= 'a') && (*ucChar* <= 'z'))

  {

​    *return* *ucChar* - 'a' + 10;

  }



  *if* (*ucChar* >= 'A')

  {

​    *return* *ucChar* - 'A' + 10;  

  }

  *else*

  {

​    *return* *ucChar* - '0';

  }

}





*//**************************************************

unsigned char CompareStr(unsigned char **pStr1*, unsigned char **pStr2*, unsigned char *Len*)

{

  unsigned char i;





  i = 0;

  *while* (i < *Len*)

  {

​    *if* (*(*pStr1* + i) != *(*pStr2* + i))

​    {

​      *return* FALSE;

​    }

​    i++;

  }



  *return* TRUE;

}





*//*************************************************

unsigned char StrLen(unsigned char **pStr*)

{

  unsigned char i;



  i = 0;

  *while* (*pStr*[i] != 0)

  {

​    i ++;

​    *if* (i >= 254)

​    {

​      *//字符串太长了*

​      *return* 254;

​    }

  }



  *return* i;

}



*//*************************************************

unsigned char StrLen0d0a(unsigned char **pStr*)

{

  unsigned char i;



  i = 0;

  *while* (*pStr*[i] != 0xd)

  {

​    i ++;

​    *if* (i >= 254)

​    {

​      *//字符串太长了*

​      *return* 254;

​    }

  }



  *return* i;

}





*//***********************************************

*//搜索范围包括pStrStart地址所指的字符，*

*//不包括pStrEnd所指的字符，pString必须以值0结束。*

*//搜索成功返回开始地址偏移量，所搜索字符串的开始*

*//地址为:pStrStart + i*

unsigned char SearchStr(unsigned char **pStrStart*, unsigned char **pStrEnd*, unsigned char **pString*)

{

  unsigned char i, ucStrLen;



  ucStrLen = StrLen(*pString*);

​    

  *if* (*pStrEnd* <= *pStrStart*)

  {

​    *return* STR_SEARCH_FAIL;

  }

  

  *if* ((*pStrEnd* - *pStrStart*) < ucStrLen)

  {

​    *return* STR_SEARCH_FAIL;

  }



  i = 0;

  *while* ((*pStrStart* + i) <= (*pStrEnd* - ucStrLen))

  {

​    *if* (CompareStr(*pStrStart* + i, *pString*, ucStrLen) == TRUE)

​    {

​      *return* i;

​    }

​    i ++;

​    

​    *if* (i >= 254)

​    {

​      *return* STR_SEARCH_FAIL;

​    }

  }

  *return* STR_SEARCH_FAIL;

}





*//*****************************************

void CopyStr(unsigned char **pDestStr*, unsigned char **pSourceStr*)

{

  unsigned char ucLen, i;



  ucLen = StrLen(*pSourceStr*);

  

  *for* (i = 0; i < ucLen; i++)

  {

​    *(*pDestStr* + i) = *(*pSourceStr* + i);

  }

  *(*pDestStr* + i) = 0;

}





*//********************************************************

unsigned char IsNumber(unsigned char **pStr*, unsigned char *len*)

{

   unsigned char i;





   i = 0;

   *while* (i != *len*)

   {

​     *if* ((*(*pStr* + i) < '0') || (*(*pStr* + i) > '9'))

​     {

​       *return* FALSE;

​     }

​     i++;

   }

   *return* TRUE;

}





*//********************************************************

uint32_t StrToValue(uint8_t **pStr*, uint8_t *num*)

{

  uint32_t value;

  uint8_t i;





  i = 0;

  value = 0;

  *while* ((i != *num*) && (*(*pStr* + i) != '.'))

  {

​    value = value * 10 + CharToValue(*(*pStr* + i));

​    i++;

  }

  *return* value;

}





*//***********************************************************

uint8_t ValueToStr(uint16_t *Value*, uint8_t **pStr*)

{

  uint8_t i, Flag, ucTmp, n;

  uint16_t Base;



  Flag = 0;

  Base = 10000;

  n = 0;

  *for* (i = 0; i < 5; i ++)

  {

​    ucTmp = *Value* / Base;

​    *Value* %= Base;

​    Base /= 10;

​    *if* (Flag == 0)

​    {

​      *if* (ucTmp != 0)

​      {

​        Flag = 1;

​        *pStr*[n ++] = ValueToChar(ucTmp);

​      }

​    }

​    *else*

​    {

​      *pStr*[n ++] = ValueToChar(ucTmp);

​    }

  }

  

  *if* (n == 0)

  {

​    n = 1;

​    *pStr*[0] = '0';

  }



  *return* n;

}





*//**************************************************************

*//比较电话号码的最后几个数字，比较长度为较短的电话号码的长度*

*//例如："8613632782450"与"13632782450"是相等的*

unsigned char CompareTelephone(unsigned char **pTelephone1*, unsigned char **pTelephone2*)

{

  unsigned char ucShort, ucLong;

 



  ucShort = min(StrLen(*pTelephone1*),StrLen(*pTelephone2*));   

  ucLong  = max(StrLen(*pTelephone1*),StrLen(*pTelephone2*));

 



  *if* (ucShort < 4)

  {

​    *return* FALSE; *//too short telephone number*

  }



  *if* (StrLen(*pTelephone1*) > StrLen(*pTelephone2*))

  {

​    *if* (CompareStr(*pTelephone1* + (ucLong - ucShort), *pTelephone2*, ucShort) 

​      == TRUE)

​    {

​      *return* TRUE;

​    }

​    *else*

​    {

​      *return* FALSE;

​    }

  }

  *else*

  {

​    *if* (CompareStr(*pTelephone2* + (ucLong - ucShort), *pTelephone1*, ucShort) 

​      == TRUE)

​    {

​      *return* TRUE;

​    }

​    *else*

​    {

​      *return* FALSE;

​    }

  }

}    





*//********************************************************

unsigned char IsHexNumber(unsigned char **pStr*, unsigned char *len*)

{

   unsigned char i;





   i = 0;

   *while* (i != *len*)

   {

​     *if* (((*(*pStr* + i) >= '0') && (*(*pStr* + i) <= '9'))

​       || ((*(*pStr* + i) >= 'A') && (*(*pStr* + i) <= 'F')))

​     {

​      *//是HEX Number*

​     }

​     *else*

​     {

​       *return* FALSE;

​     }

​     i++;

   }

   *return* TRUE;

}





*//***********************************************

*//大写字母转换成小写字母*

void BigLetterToSmallLetter(unsigned char **pStr*)

{

  unsigned char i, value;





  i = 0;

  value = *(*pStr* + i);

  *while* (value != 0)

  {

​    *if* ((value >= 'A') && (value <= 'Z'))

​    {

​      *(*pStr* + i) = value - 'A' + 'a';

​    }



​    i++;

​    value = *(*pStr* + i);

  }

}



*//***********************************************

*//小写字母转换成大写字母*

void SmallLetterToBigLetter(uint8_t **pStr*)

{

  uint8_t i, value;





  i = 0;

  value = *(*pStr* + i);

  *while* (value != 0)

  {

​    *if* ((value >= 'a') && (value <= 'z'))

​    {

​      *(*pStr* + i) = value - 'a' + 'A';

​    }



​    i++;

​    value = *(*pStr* + i);

  }

}



*//--------------10进制转16进制----------------------------------------*

uint8_t NumberValueToHexStr(uint16_t *number*,uint8_t **s*)

{

  unsigned char i,n;

​     

  *s*[0] = (unsigned char)(*number* / 256); 

  *s*[1] = (unsigned char)(*number* / 16);

  *s*[2] = (unsigned char)(*number* % 16);



  *if*(*number* > 256)

  {  

​    *s*[1] = *s*[1] - ((*number* / 256) * 16);

  }



  *for*(i = 0; i < 3; i++)

  {

​    *if*((*s*[i] > 9) && (*s*[i] <= 15)) *//解决上报乱码*

​    {

​      *s*[i] = *s*[i] - 10 + 'A';

​    }

​    *else*

​    {

​      *s*[i] = *s*[i] + '0';

​    }

  }



  *if*(*number* < 4096)

  {

​    n = 3;

  }

  *else* *if*(*number* < 256)

  {

​    n = 2;

  }

  *else* 

  {

​    n = 1;

  }   



  *return* n;

}  



*/***********************************************************************************************************

** 函数名 : Search_Char_Number*

** 描  述 : 在给定字符串中搜索指定字符*

** 输  入 : Pbuf  ---要操作的字符串,即被搜索字符串的首地址*

**      Length---被搜索的字符串的长度,可以理解成从首地址开始,最多只查多少个字节*

**      c   ---指定需要搜索的字符*

** 返回值 : 搜索成功，返回字符c在给定字符串中所在的偏移位置，否则，超过此长度仍未搜索到指定字符*

**      或搜索到/0结束符，返回0*

************************************************************************************************************/*

uint8_t Search_Char_Number(uint8_t **Pbuf*, uint16_t *Length*, char *c*)

{

  uint16_t i;

  uint8_t  j = 0;



  *for*(i = 0; i < *Length*; i++)

  {

​    *if*(*Pbuf*[i] == 0x0)

​     *break*;

​    *if*(*Pbuf*[i] == *c*)

​     j++;

  }

  *return* j;

}



*/**********************************************************************************************************

*函数名 : GetSubStr*

*功能:从一串字符串中取出第N个字符串，*

*传入:*

   *INT8U \* str   要操作的字符串,即被查找字符串的首地址*

   *INT8U  strlen  被查找的字符串的长度,可以理解成从首地址开始,最多只查多少个字节*

   *INT8U  index  查找第几个字符串,也是从要操作的字符串的\*str地址开始*

   *INT8U \* start  查找到的字符串的起点,若没有找到,则为0*

   *INT8U \* len   查找到的字符串的长度,若没有找到,则为0*

*返回:无*

*说明:每一个字符串之间用","号分隔开，且规定最后一个字符必须是","号*

***********************************************************************************************************/*

void GetSubStr(uint8_t * *str*,uint16_t *strlen*,uint8_t *index*,uint16_t * *start*,uint8_t * *len*)

{

 uint16_t i,istart = 0x00;

 uint8_t c, t = 0x00, ilen = 0x00;





 *for*( i = 0 ; i < *strlen* ; i++)

 {

  c = *str*[i];

  *if* (c == ',' || i == *strlen* - 1)

  {

   t++;

   *if* (t == *index*)

   {

​    \* *start* = istart;

​    *if* (c == ',')

​     \* *len* = ilen;

​    *else*

​     \* *len* = ilen + 1;

​    *return*;

   }

   *else*

   {

​    istart = i + 1;

​    ilen   = 0x00;

   }

  }

  *else*

   ilen++;

 }

}



*/**********************************************************************************************************

*函数名 : GetSubStrToUINT*

*功能:从一串字符串中取出第N个字符串，然后将其转成UINT型值，只能转换0--9的字符串*

*传入:*

   *INT8U \* str   要操作的字符串,即被查找字符串的首地址*

   *INT8U  strlen  被查找的字符串的长度,可以理解成从首地址开始,最多只查多少个字节*

   *INT8U  index  查找第几个字符串,也是从要操作的字符串的\*str地址开始*

*返回:*

   *INT16U       转换后的值*

*说明:每一个字符串之间用","号分隔开，且规定最后一个字符必须是","号*

***********************************************************************************************************/*

uint16_t GetSubStrToUINT(uint8_t * *str*,uint16_t *strlen*,uint8_t *index*)

{

 uint16_t temp = 0x00;

 uint16_t istart;

 uint8_t ilen,i,c;





 GetSubStr(*str*,*strlen*,*index*,&istart,&ilen);

 *if* (ilen != 0x00)

 {

  *for*( i = 0 ; i < ilen ; i++)

  {  

   c = *str*[istart + i];

   *if* (c <= '9' && c >= '0')

​    temp = temp * 10 + (*str*[istart + i] - 0x30);

   *else*

​    *break*;

  }

 }

 *return* temp;

}



int hexCharToValue(const char *ch*){

 int result = 0;

 *//获取16进制的高字节位数据*

 *if*(*ch* >= '0' && *ch* <= '9'){

  result = (int)(*ch* - '0');

 }

 *else* *if*(*ch* >= 'a' && *ch* <= 'z'){

  result = (int)(*ch* - 'a') + 10;

 }

 *else* *if*(*ch* >= 'A' && *ch* <= 'Z'){

  result = (int)(*ch* - 'A') + 10;

 }

 *else*{

  result = -1;

 }

 *return* result;

}



int hexToStr(char **hex*, char **ch*)

{

 int high,low;

 int tmp = 0;

 *if*(*hex* == NULL || *ch* == NULL){

  *return* -1;

 }



 *if*(strlen(*hex*) %2 == 1){

  *return* -2;

 }



 *while*(**hex*){

  high = hexCharToValue(**hex*);

  *if*(high < 0){

   **ch* = '\0';

   *return* -3;

  }

  *hex*++; *//指针移动到下一个字符上*

  low = hexCharToValue(**hex*);

  *if*(low < 0){

   **ch* = '\0';

   *return* -3;

  }

  tmp = (high << 4) + low;

  **ch*++ = (char)tmp;

  *hex*++;

 }

 **ch* = '\0';

 *return* 0;

}



char valueToHexCh(const int *value*)

{

 char result = '\0';

 *if*(*value* >= 0 && *value* <= 9){

  result = (char)(*value* + 48); *//48为ascii编码的‘0’字符编码值*

 }

 *else* *if*(*value* >= 10 && *value* <= 15){

  result = (char)(*value* - 10 + 65); *//减去10则找出其在16进制的偏移量，65为ascii的'A'的字符编码值*

 }

 *else*{

  ;

 }



 *return* result;

}



int strToHex(char **ch*, char **hex*)

{

 int high,low;

 int tmp = 0;

 *if*(*ch* == NULL || *hex* == NULL){

  *return* -1;

 }



 *if*(strlen(*ch*) == 0){

  *return* -2;

 }



 *while*(**ch*){

  tmp = (int)**ch*;

  high = tmp >> 4;

  low = tmp & 15;

  **hex*++ = valueToHexCh(high); *//先写高字节*

  **hex*++ = valueToHexCh(low); *//其次写低字节*

  *ch*++;

 }

 **hex* = '\0';

 *return* 0;

}