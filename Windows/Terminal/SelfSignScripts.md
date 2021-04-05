# How to selfsign a script to run it in windows powershell

I was following the tutorial on [how to install the Powerline in my Windows Terminal application](https://docs.microsoft.com/en-us/windows/terminal/tutorials/powerline-setup#customize-your-powershell-prompt) when I stumbled upon an error that told me I couldn't run the script the tutorial proposed me.

![OriginalNotSignedError](https://user-images.githubusercontent.com/37411225/113568696-4e778c00-9611-11eb-94f5-f0d44fea9375.PNG)

So I learned that Microsoft Powershell has execution policies which determine when a script can be run in the system and at what conditions.

## But what policy do I have on my machine?

By running the command `Get-ExecutionPolicy -List` you get the policies active on your machine:

![MyExecutionPolicies](https://user-images.githubusercontent.com/37411225/113568719-5afbe480-9611-11eb-9830-3f798e78b7e5.PNG)

In my case `All signed` is used on my machine to run scripts only if they are digitally signed, more info on execution policies [here](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.1).
Indeed if you had the same error you may find that your execution policy does not permit the execution of not signed scripts, either for downloaded and/or your handwritten ones.

That can be fixed in two main ways:
1. Allowing a more permissive execution policy, which may not be doable if you are working on your company machine
2. Self signing your script generating a personal certificate, leaving inhaltered the executionPolicies configurations :slightly_smiling_face:

---

## Change policy:

If you really want to change execution policies I recommend to use the `RemoteSigned` policy, which allows the execution of scripts only if written on your machine.

You can change your execution policies by typing:
`-ExecutionPolicy RemoteSigned -Scope LocalMachine`
(With the `-Scope` parameter you can decide where the execution will be applied)

---

## Add a self signed certificate to your script

Follow the next steps by typing the commands on your powershell as administrator:

1. Create a certificate
<details>
<summary>&lt;your machine name&gt;:</summary>

```
    Click on the Start button.
    In the search box, type Computer.
    Right click on This PC within the search results and select Properties.
    Under Computer name, domain, and workgroup settings you will find the computer name listed.
```
![MachineName](https://user-images.githubusercontent.com/37411225/113568752-736bff00-9611-11eb-85c1-035d96de9302.PNG)
</details>

```bash
New-SelfSignedCertificate -Type 'CodeSigningCert' -DnsName '<your machine name>' -CertStoreLocation cert:\LocalMachine\My
```
> !Important-> The `subject` column is very important to quickly recognize your new certificate in the following command output

2. Check if the certificate is present in the specified path
```bash
Get-ChildItem cert:\LocalMachine\My -codesigning
```

3. Save the certificate path on a variable
```bash
$cert = @(Get-ChildItem cert:\LocalMachine\My -codesigning)[<the position of your certificate in the previous command output>]
```

4. Use the variable to associate the certificate to your script
```bash
Set-AuthenticodeSignature <The script path in your system> $cert
```

The commands in the powershell should look something like the following:

![ValidProcedureOnPowerShellAdmin](https://user-images.githubusercontent.com/37411225/113568785-867ecf00-9611-11eb-827a-65bc860c84ea.PNG)


Now you should be able to run your script 🙂
