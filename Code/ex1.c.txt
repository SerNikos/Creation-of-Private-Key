//NIKOLAOS SERGIS 18390173 PADA EX_1

#include <stdio.h> //basic library for input/output 
#include <openssl/bn.h> //library that helps the computer to deal with big numbers and not cause overflow
#define NBITS 128 //constant for bn, (in real life problems that number would be at least 512 bits)

//Function that prints a bn number
void printBN(char *msg, BIGNUM * a)
{

 /* Use BN_bn2hex(a) for hex string
 * Use BN_bn2dec(a) for decimal string */
 char * number_str = BN_bn2hex(a);
 printf("%s %s\n", msg, number_str);
 OPENSSL_free(number_str);

}

int main ()
{

//Varables declaration
 BN_CTX *ctx = BN_CTX_new();// a temporary struct to help with the computational process of large numbers
 BIGNUM *p = BN_new();//p is a first number, we will multiply with q in order to find n
 BIGNUM *q = BN_new(); //q is a first number, we will multiply with p in order to find d
 BIGNUM *e = BN_new(); //public key
 BIGNUM *n = BN_new(); //n will be the result of the multiplication of two first numbers p*q
 BIGNUM *d = BN_new(); //private key
 BIGNUM *i = BN_new(); //varable will be used for the d computation 
 BIGNUM *M = BN_new(); //the message
 BIGNUM *res1 = BN_new(); //varable will be used for the d computation 
 BIGNUM *res2 = BN_new(); //varable will be used for the d computation 
 BIGNUM *res3 = BN_new(); //varable will be used for the d computation 
 BIGNUM *rese = BN_new(); //varable for encryption
 BIGNUM *resd = BN_new(); //varable for dencryption

//Initialize p, q, e, i
 BN_hex2bn(&p, "953AAB9B3F23ED593FBDC690CA10E703");
 BN_hex2bn(&q, "C34EFC7C4C2369164E953553CDF94945");
 BN_hex2bn(&e, "0D88C3");
 BN_hex2bn(&i, "1");

//Calculate n = p * q
 BN_mul(n, p, q, ctx);

//Calculate res1,res2, res3
 BN_sub(res1, p, i);
 BN_sub(res2, q, i);
 BN_mul(res3, res1, res2, ctx); // (p-1)*(q-1) 

//Calculate d private key
 BN_mod_inverse(d, e, res3, ctx); 

//Prints peivate key d
 printBN("d = ", d);

 return 0;
 
}