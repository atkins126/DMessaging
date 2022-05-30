# DMessaging

A messaging lib for Delphi

## Example
```pascal
program DMessagingApp;

{$APPTYPE CONSOLE}

{$R *.res}

uses
  System.SysUtils,
  DMessaging in 'Src\DMessaging.pas',
  DMessaging.Channel in 'Src\DMessaging.Channel.pas';

const
  MSG_NEW_USER = 'NewUser';
  
 type
   TUser = class
     Firstname, Lastname: string;
   end;

begin
  try
    TMessaging.RegisterAnonymousSubscriber<TUser>(
        procedure (const MessageID: string; Arg: TUser)
          begin
            if MessageID = MSG_NEW_USER then
              Writeln('New user: ', Arg.Firstname, ' ', Arg.Lastname);
          end
    );

    TMessaging.SendThenFree(
        MSG_NEW_USER,
        TUser.Create('Foo', 'Bar')
    );
  except
    on E: Exception do
      Writeln(E.ClassName, ': ', E.Message);
  end;
  ReadLn;
end.

```

Output:
```
New user: Foo Bar
```
