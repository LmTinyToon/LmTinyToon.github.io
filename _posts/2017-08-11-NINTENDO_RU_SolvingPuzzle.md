---
title: Задача от Nintendo и теория конечных групп
layout: default
---

    #include <iostream>
    #include <iomanip>
    
    using namespace std;
    
    int main()
    {
      int size;
    
      cin >> size;
    
      unsigned int* a = new unsigned int[size / 16]; // <- input tab to encrypt
      unsigned int* b = new unsigned int[size / 16]; // <- output tab
     
      for (int i = 0; i < size / 16; i++) {   // Read size / 16 integers to a
        cin >> hex >> a[i];
      }
    
      for (int i = 0; i < size / 16; i++) {   // Write size / 16 zeros to b
        b[i] = 0;
      }	
     
      for (int i = 0; i < size; i++)
        for (int j = 0; j < size; j++) {
          b[(i + j) / 32] ^= ( (a[i / 32] >> (i % 32)) &
    		       (a[j / 32 + size / 32] >> (j % 32)) & 1 ) << ((i + j) % 32);   // Magic centaurian operation
      }
     
      for(int i = 0; i < size / 16; i++) {
        if (i > 0) {
          cout << ' ';
        }
        cout << setfill('0') << setw(8) << hex << b[i];       // print result
      }
      cout << endl;
    
     /* 
        Good luck humans     
     */
      return 0;
    }
    
    
Тестовые наборы:

1) Ввод (32 бита): 

        32
        000073af 00000000
    
   Вывод:
   
        00000001 000073af
        00000083 000000e5
        000000e5 00000083
        000073af 00000001
        
        
2) Ввод (32 бита): 

        32
        738377c1 00000000
    
   Вывод:
   
        00000001 738377c1
        0000b0c5 0000cd55
        0000cd55 0000b0c5
        738377c1 00000001        
        
3) Ввод (32 бита): 

        32
        46508fb7 6677e201
    
   Вывод:
   
        b0c152f9 ebf2831f
        ebf2831f b0c152f9 
        
4) Ввод (32 бита): 

        64
        f3268b49 661859eb 0b324559 65ee6bda
    
   Вывод:
   
        0cf5c2bf 9aba68ef c18fb79b de70eef7
        c18fb79b de70eef7 0cf5c2bf 9aba68ef
        
5) Ввод (128 бит): 

        128
        a91db473 fcea8db4 f3bb434a 8dba2f16 51abc87e 92c44759 5c1a16d3 6111c6f4
    
   Вывод:
   
        a30d28bd bda19675 3f95d074 b6f69434 c58f4047 d73fe36a 24be2846 e2ebe432
        c58f4047 d73fe36a 24be2846 e2ebe432 a30d28bd bda19675 3f95d074 b6f69434
   
    