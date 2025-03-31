# Expanding EC2 Volume from 8 GiB to 12 GiB

## Overview
This document provides step-by-step instructions to expand an **EBS volume** attached to an **EC2 instance** from **8 GiB to 12 GiB** using the **AWS Console** and ensure the new space is available for immediate use.

---

## Steps to Expand the Volume

### **1. Identify the Attached Volume**
1. Log in to the **AWS Management Console**.
2. Navigate to **EC2 Dashboard**.
3. Select **Instances** and find the instance named **"ec2"**.
4. In the **Storage** section, locate the attached **EBS Volume ID** and click on it.

---

### **2. Modify the Volume Size**
1. In the **EBS Volumes** section, click on the volume identified in the previous step.
2. Click **Modify Volume**.
3. Change the **Size** from **8 GiB** to **12 GiB**.
4. Click **Modify** and confirm the change.

---

## Steps to Reflect Changes in the EC2 Instance

### **For Linux Instances**
1. **Connect to the EC2 instance via SSH**:
   ```sh
   ssh -i your-key.pem ec2-user@your-instance-ip
   ```
2. **Verify the disk size**:
   ```sh
   lsblk
   ```
   - The root volume (e.g., `/dev/xvda1` or `/dev/nvme0n1p1`) should now show **12 GiB**, but the filesystem may still be using only **8 GiB**.

3. **Expand the filesystem**:
   - For **ext4** (Amazon Linux, Ubuntu, RHEL, etc.):
     ```sh
     sudo growpart /dev/xvda 1
     sudo resize2fs /dev/xvda1
     ```
   - For **xfs** (Amazon Linux 2, RHEL 8+):
     ```sh
     sudo growpart /dev/xvda 1
     sudo xfs_growfs /
     ```
   - If using **NVMe disks** (e.g., `nvme0n1`):
     ```sh
     sudo growpart /dev/nvme0n1 1
     sudo resize2fs /dev/nvme0n1p1  # For ext4
     sudo xfs_growfs /              # For xfs
     ```

4. **Confirm the new size is available**:
   ```sh
   df -h /
   ```
   - The root partition should now show **12 GiB**.

---

### **For Windows Instances**
1. **Connect via RDP** to the Windows EC2 instance.
2. Open **Disk Management** (`diskmgmt.msc`).
3. Locate the **C: drive** (root volume), right-click, and select **Extend Volume**.
4. Follow the wizard to allocate the new space.
5. Verify the new disk size.

---

## **Verification**
- Run `df -h /` (Linux) or check **Disk Management** (Windows) to confirm the expansion.
- No reboot is required; the expanded space is immediately available.

---

## **Conclusion**
By following this guide, you successfully increased the **EBS volume size** from **8 GiB to 12 GiB**, ensuring the additional space is accessible without disrupting ongoing activities.

---

### **Author**
Kranthi Vodnala
