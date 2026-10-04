## Persistent volume or PV is look like LVM in linux you can create a block space with PV and it's look like your physical volume and you can give a slice of your PV and we call it PVC or persistent volume claim and it's look like your LV in LVM and you can extend it base on your PV and PVC is look like resizable volume and you can resize it in a second

### 1- Difference model of PV and PVC
`Read only many (ROX)` **:** **Mount read only on whole nodes**
`Read Write once (RWO)` **:** **Create a PV only on a specific node**
`Read Write many (RWX)` **:** **Mount read and write on whole nodes**
`Read Write once pod (RWOP)` **:** **Only read and write on a specific pod**

#### Note: You can choose for example RWX for your PV but change your PVC to ROX if you don't configure it by yourself your PVC will use PV model