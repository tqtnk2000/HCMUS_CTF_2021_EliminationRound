---
layout: post
title: naminh's writeups
---

### AndroidRev

We open file in jadx to decompile this file. 

![TreeFolder](/assets/img/Capture.PNG)

Open MainActivity we get this code.

~~~
package com.hcmusctf.androidrev;

import android.os.Bundle;
import android.text.Editable;
import android.text.TextWatcher;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.internal.view.SupportMenu;

public class MainActivity extends AppCompatActivity {
    TextView mResultWidget = null;

    /* access modifiers changed from: protected */
    @Override // androidx.activity.ComponentActivity, androidx.core.app.ComponentActivity, androidx.appcompat.app.AppCompatActivity, androidx.fragment.app.FragmentActivity
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        final EditText flagWidget = (EditText) findViewById(R.id.flag);
        final TextView resultWidget = (TextView) findViewById(R.id.result);
        this.mResultWidget = resultWidget;
        flagWidget.addTextChangedListener(new TextWatcher() {
            /* class com.hcmusctf.androidrev.MainActivity.AnonymousClass1 */

            public void beforeTextChanged(CharSequence s, int start, int count, int after) {
            }

            public void onTextChanged(CharSequence s, int start, int before, int count) {
                MainActivity.this.mResultWidget.setText("");
            }

            public void afterTextChanged(Editable s) {
            }
        });
        ((Button) findViewById(R.id.checkflag)).setOnClickListener(new View.OnClickListener() {
            /* class com.hcmusctf.androidrev.MainActivity.AnonymousClass2 */

            public void onClick(View v) {
                int color;
                String msg;
                if (FlagChecker.checkFlag(MainActivity.this, flagWidget.getText().toString())) {
                    msg = "Valid flag!";
                    color = MainActivity.this.getApplicationContext().getResources().getColor(R.color.green);
                } else {
                    msg = "Invalid flag";
                    color = SupportMenu.CATEGORY_MASK;
                }
                resultWidget.setText(msg);
                resultWidget.setTextColor(color);
            }
        });
    }
}
~~~
Easy to know, this program checks our flag in FlagChecker. Open file FlagChecker.

~~~
package com.hcmusctf.androidrev;

import android.content.Context;
import android.util.Base64;
import android.util.Log;
import java.security.MessageDigest;
import java.util.HashSet;
import java.util.Set;

public class FlagChecker {
    public static boolean checkFlag(Context ctx, String flag) {
        if (!flag.startsWith("HCMUS-CTF{") || !flag.endsWith("}")) {
            return false;
        }
        String core = flag.substring(10, 42);
        if (core.length() != 32) {
            return false;
        }
        String[] ps = core.split(foo());
        if (ps.length != 5 || !bim(ps[0]) || !bum(ps[2]) || !bam(ps[4]) || !core.replaceAll("[A-Z]", "X").replaceAll("[a-z]", "x").replaceAll("[0-9]", " ").matches("[A-Za-z0-9]+.       .[A-Za-z0-9]+.[Xx ]+.[A-Za-z0-9 ]+")) {
            return false;
        }
        char[] syms = new char[4];
        int[] idxs = {15, 23, 29, 34};
        Set<Character> chars = new HashSet<>();
        for (int i = 0; i < syms.length; i++) {
            syms[i] = flag.charAt(idxs[i]);
            chars.add(Character.valueOf(syms[i]));
        }
        int sum = 0;
        for (char c : syms) {
            sum += c;
        }
        if (sum != 180 || chars.size() != 1 || !me(ctx, dh(gs(ctx.getString(R.string.ct1), ctx.getString(R.string.k1)), ps[0]), ctx.getString(R.string.t1)) || !me(ctx, dh(gs(ctx.getString(R.string.ct2), ctx.getString(R.string.k2)), ps[1]), ctx.getString(R.string.t2)) || !me(ctx, dh(gs(ctx.getString(R.string.ct3), ctx.getString(R.string.k3)), ps[2]), ctx.getString(R.string.t3)) || !me(ctx, dh(gs(ctx.getString(R.string.ct4), ctx.getString(R.string.k4)), ps[3]), ctx.getString(R.string.t4)) || !me(ctx, dh(gs(ctx.getString(R.string.ct5), ctx.getString(R.string.k5)), ps[4]), ctx.getString(R.string.t5)) || !me(ctx, dh(gs(ctx.getString(R.string.ct6), ctx.getString(R.string.k6)), flag), ctx.getString(R.string.t6))) {
            return false;
        }
        return true;
    }

    private static boolean bim(String s) {
        return s.matches("^[a-z]+$");
    }

    private static boolean bum(String s) {
        return s.matches("^[A-Z]+$");
    }

    private static boolean bam(String s) {
        return s.matches("^[0-9]+$");
    }

    private static String dh(String hash, String s) {
        try {
            MessageDigest md = MessageDigest.getInstance(hash);
            md.update(s.getBytes());
            return toHexString(md.digest());
        } catch (Exception e) {
            return null;
        }
    }

    private static String toHexString(byte[] bytes) {
        StringBuilder hexString = new StringBuilder();
        for (byte b : bytes) {
            String hex = Integer.toHexString(b & 255);
            if (hex.length() == 1) {
                hexString.append('0');
            }
            hexString.append(hex);
        }
        return hexString.toString();
    }

    public static String foo() {
        String s = "Vm0wd2QyVkZNVWRYV0docFVtMVNWVmx0ZEhkVlZscDBUVlpPVmsxWGVIbFdiVFZyVm0xS1IyTkliRmRXTTFKTVZsVmFWMVpWTVVWaGVqQTk=";
        for (int i = 0; i < 10; i++) {
            s = new String(Base64.decode(s, 0));
        }
        return s;
    }

    private static String gs(String a, String b) {
        String s = "";
        for (int i = 0; i < a.length(); i++) {
            s = s + Character.toString((char) (a.charAt(i) ^ b.charAt(i % b.length())));
        }
        return s;
    }

    private static boolean me(Context ctx, String s1, String s2) {
        try {
            return ((Boolean) s1.getClass().getMethod(r(ctx.getString(R.string.m1)), Object.class).invoke(s1, s2)).booleanValue();
        } catch (Exception e) {
            Log.e("HCMUS-CTF", "Exception: " + Log.getStackTraceString(e));
            return false;
        }
    }

    public static String r(String s) {
        return new StringBuffer(s).reverse().toString();
    }
}
~~~

The flag is: **HCMUS-CTF{something here}** and length of the flag is 42. Length(something here) = 32.

Character at 15, 23, 29, 34 are '-'. Now flag has format **HCMUS-CTF{s[1]-s[2]-s[3]-s[4]-s[5]}**. Length(s[1]) = 5. Length(s[2]) = 7. Length(s[3]) = 5. Length(s[4]) = 4. Length(s[1]) = 7.

s[1] contains only character (a-z), s[3] contains character (A-Z), s[5] contains character (0, 9).

Then checks this code

~~~
	!me(ctx, dh(gs(ctx.getString(R.string.ct1), ctx.getString(R.string.k1)), ps[0]), ctx.getString(R.string.t1))
~~~
In Android, each string is represented by a number. We can find _resources.arsc/res/values/string_. This code get string in R.string.ct1 and R.string.k1 and encrypted by gs(). A string returned by gs is "MD5". I think string encrypted by MD5 algorithm. We can decrypt online.

Do the same, we can get flag is
```
HCMUS-CTF{peppa-9876543-BAAAM-A1z9-3133337}
```

### Mixed-VM

Open file in IDA. Main function looks like

```cpp
__int64 __fastcall main(int a1, char **a2, char **a3)
{
  void *ptr; // [rsp+18h] [rbp-8h]

  ptr = malloc(32uLL);
  sub_7E6(ptr);
  sub_839(ptr);
  sub_C75(ptr);
  free(ptr);
  return 0LL;
}
```

ptr is a pointer has 32bytes. Go to sub_7E6(ptr).

```cpp
__int64 __fastcall sub_7E6(__int64 a1)
{
  __int64 result; // rax

  *(_DWORD *)a1 = 0;
  *(_DWORD *)(a1 + 4) = 0;
  *(_QWORD *)(a1 + 8) = malloc(0x1000uLL);
  *(_QWORD *)(a1 + 16) = &unk_202020;
  result = a1;
  *(_DWORD *)(a1 + 24) = 0;
  return result;
}
```
This is the constructor for a struct. We define new struct for ptr. Struct has size 32bytes.

```cpp
struct dd
{
    int inputData;
    int unk2;
    char* bufDataInput;
    char* opCode;
    int unk5;
    int unk6;
};
```
Type of ptr is dd*. Now we change type of data in IDA. We have a new code in sub_839(ptr)

```cpp
unsigned __int64 __fastcall sub_839(dd *a1)
{
  int *v1; // rax
  int *v2; // rax
  int *v3; // rax
  int *v4; // rax
  char *v5; // rdx
  char *v6; // rdx
  int *v7; // rax
  int *v8; // rax
  int buf; // [rsp+1Ch] [rbp-34h] BYREF
  int v11; // [rsp+20h] [rbp-30h]
  unsigned int v12; // [rsp+24h] [rbp-2Ch]
  int v13; // [rsp+28h] [rbp-28h]
  int v14; // [rsp+2Ch] [rbp-24h]
  int v15; // [rsp+30h] [rbp-20h]
  int v16; // [rsp+34h] [rbp-1Ch]
  int v17; // [rsp+38h] [rbp-18h]
  int v18; // [rsp+3Ch] [rbp-14h]
  char *v19; // [rsp+40h] [rbp-10h]
  unsigned __int64 v20; // [rsp+48h] [rbp-8h]

  v20 = __readfsqword(0x28u);
  v19 = a1->bufDataInput;
  v11 = 0;
  while ( !v11 )
  {
    v12 = *(_DWORD *)a1->opCode;    // Load 4 bytes in opCode
    if ( v12 == -2044552814 )
    {
      v19 += 4;
      *(_DWORD *)v19 = a1->inputData;
      a1->opCode += 4;
    }
    else if ( v12 > 0x86229992 )
    {
      if ( v12 == -861274094 ) // Compare value encrypted with value loads in opcode
      {
        a1->unk5 = a1->inputData == a1->unk2;
        a1->opCode += 4;
      }
      else if ( v12 > 0xCCAA0012 )
      {
        switch ( v12 )
        {
          case 0xEFEFEFEF:     // Write 4 bytes.
            buf = *((_DWORD *)a1->opCode + 1);
            write(1, &buf, 4uLL);
            a1->opCode += 8;
            break;
          case 0xFFFFFFFF:
            v11 = 1;
            break;
          case 0xCCCCCCCC: // Read 4 bytes.
            read(0, a1, 4uLL);
            a1->opCode += 4;
            break;
        }
      }
      else
      {
        switch ( v12 )
        {
          case 0x98762222:   // Encrypt 
            v7 = (int *)v19;
            v19 -= 4;
            v13 = *v7;
            v8 = (int *)v19;
            v19 -= 4;
            v14 = *v8;
            v15 = v13 + v14;
            v19 += 4;
            *(_DWORD *)v19 = v13 + v14;
            a1->opCode += 4;
            break;
          case 0xABCDEF00:    // Load data in opcode
            buf = *((_DWORD *)a1->opCode + 1);
            a1->unk2 = buf;
            a1->opCode += 8;
            break;
          case 0x92660114:
            v2 = (int *)v19;
            v19 -= 4;
            a1->unk2 = *v2;
            a1->opCode += 4;
            break;
        }
      }
    }
    else if ( v12 == 572732228 )
    {
      buf = *((_DWORD *)a1->opCode + 1);
      if ( a1->unk5 )
        v5 = &a1->opCode[4 * buf];
      else
        v5 = a1->opCode + 8;
      a1->opCode = v5;
    }
    else if ( v12 > 0x22233344 )
    {
      switch ( v12 )
      {
        case 0x44333222u:
          buf = *((_DWORD *)a1->opCode + 1);
          if ( a1->unk5 )
            v6 = a1->opCode + 8;
          else
            v6 = &a1->opCode[4 * buf];
          a1->opCode = v6;
          break;
        case 0x83660101:         // Encrept
          v3 = (int *)v19;
          v19 -= 4;
          v16 = *v3;
          v4 = (int *)v19;
          v19 -= 4;
          v17 = *v4;
          v18 = v17 ^ v16;
          v19 += 4;
          *(_DWORD *)v19 = v17 ^ v16;
          a1->opCode += 4;
          break;
        case 0x34535888u:
          buf = *((_DWORD *)a1->opCode + 1);
          v19 += 4;
          *(_DWORD *)v19 = buf;
          a1->opCode += 8;
          break;
      }
    }
    else if ( v12 == 286335522 )
    {
      v1 = (int *)v19;
      v19 -= 4;
      a1->inputData = *v1;
      a1->opCode += 4;
    }
    else if ( v12 == 305419896 )
    {
      buf = *((_DWORD *)a1->opCode + 1);
      a1->inputData = buf;
      a1->opCode += 8;
    }
  }
  return __readfsqword(0x28u) ^ v20;
}
```

Then use the debugger to know what does this function do? This function is a virtual machine and the opcode has a size 4bytes, opcode has address 0x202020 in IDA. The function reads 4 characters then encrypts. Compare it with value load in data. The correct way has two values equal. We can decompile with the correct way.

We can decompile based on how the program runs correctly. Can you code decompiler? 

FLAG
```
HCMUS-CTF{i_like_using_vm_to_protect_my_program}
```

