# Query Active Directory Using Powershell

### **Search in active directory**

```powershell
$LDAPSEARCH = New-Object System.DirectoryServices.DirectorySearcher
$LDAPSEARCH.FindAll()
```

---

### **Filter by domain name**

```powershell
$LDAPSEARCH = New-Object System.DirectoryServices.DirectorySearcher 
$LDAPSEARCH.SearchRoot = "LDAP://DC=fabric,DC=local"
$LDAPSEARCH.FindAll()
```

---

### **Filter by acount name**

```powershell
$LDAPSEARCH = New-Object System.DirectoryServices.DirectorySearcher 
$LDAPSEARCH.SearchRoot = "LDAP://DC=fabric,DC=local"
$LDAPSEARCH.Filter = "(objectclass=user)"
$LDAPSEARCH.FindAll()
```

---

### **Get list users information **

```powershell
$LDAPSEARCH = New-Object System.DirectoryServices.DirectorySearcher 
$LDAPSEARCH.SearchRoot = "LDAP://DC=TECH,DC=LOCAL"
$LDAPSEARCH.Filter = "(objectclass=user)"
$LDAPSEARCH_RESULTS = $LDAPSEARCH.FindAll()
$RESULTS = foreach ($LINE in $LDAPSEARCH_RESULTS) {
$LINE_ENTRY = $LINE.GetDirectoryEntry()
$LINE_ENTRY | Select-Object @{ Name = "sAMAccountName";  Expression = { $_.sAMAccountName }},
@{ Name = "userPrincipalName";  Expression = { $_.userPrincipalName }},
@{ Name = "name";  Expression = { $_.name }},
@{ Name = "distinguishedName"; Expression = { $_.distinguishedName | Select-Object -First 1 }}
}
$RESULTS
```

---

### **Get list group information**

```powershell
$LDAPSEARCH = New-Object System.DirectoryServices.DirectorySearcher 
$LDAPSEARCH.SearchRoot = "LDAP://DC=fabric,DC=local"
$LDAPSEARCH.Filter = "(objectclass=group)"
$LDAPSEARCH.FindAll()
```

---

### **Get membership of user inside group**

```powershell
$samAccountName = "TARASOL Users"
# $samAccountName = "TARASOL Admins"
$group = ([adsisearcher]"samAccountName=$samAccountName").FindAll()
$group.Properties.member
```

```powershell
$Group = [ADSI]"LDAP://CN=TARASOL Users,OU=HHPO Tarasol Groups,DC=fabric,DC=local"
$Members = $Group.Member | ForEach-Object {[ADSI]"LDAP://$_"}

echo $Members
```
