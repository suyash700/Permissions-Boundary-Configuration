## Permissions-Boundary-Configuration
IAM Permission Boundary Setup — Step-by-Step Guide

This guide explains how to create a Permission Boundary Policy in AWS, attach it to an IAM user, and test that the boundary restricts permissions even when the identity policy is permissive.

## Step 1 — Create IAM User Without Permission Boundary

1.1 Create IAM User

Go to IAM Dashboard → Users → Add user

User name: pec-user-1

Access type: AWS Management Console access
<img width="1919" height="975" alt="Screenshot 2025-10-07 214655" src="https://github.com/user-attachments/assets/1a432819-9c8b-4df8-97e7-470c7f9703d5" />


1.2 Attach Identity Policy

Attach the managed policy: IAMFullAccess
<img width="1919" height="978" alt="Screenshot 2025-10-07 215909" src="https://github.com/user-attachments/assets/1ed19ff4-78d9-4ddc-888a-21857920931d" />


1.3 Login and Test

Login as pec-user-1
<img width="1919" height="978" alt="Screenshot 2025-10-07 215230" src="https://github.com/user-attachments/assets/daa6ae24-8695-4dea-a710-d139a1b5d69b" />

<img width="1919" height="1079" alt="Screenshot 2025-10-07 215314" src="https://github.com/user-attachments/assets/10021c7a-c225-40b2-989f-8647cbed22da" />

<img width="1919" height="1030" alt="Screenshot 2025-10-07 215404" src="https://github.com/user-attachments/assets/883eb4ef-fe34-4cea-97dc-7c241bdc16a9" />


Attempt to delete another IAM user → deletion succeeds
<img width="1919" height="980" alt="Screenshot 2025-10-07 220251" src="https://github.com/user-attachments/assets/91d92fd5-eb11-4564-a001-01545ca71a08" />


<img width="1919" height="980" alt="Screenshot 2025-10-07 220311" src="https://github.com/user-attachments/assets/569bd526-65da-4f4b-a06a-a133e096735c" />

Observation: Without a permission boundary, the identity policy grants full permissions.

## Step 2 — Create a Custom Permission Boundary Policy
2.1 Create Custom Policy

Go to IAM Dashboard → Policies → Create policy

Use the following JSON:

<img width="1919" height="979" alt="Screenshot 2025-10-07 214748" src="https://github.com/user-attachments/assets/eed6ffad-7ea0-4798-89b6-36be1183bb36" />

2.2 Name Policy

Policy name: my-iam-policy

This policy denies deletion of IAM users while allowing other actions.
<img width="1919" height="976" alt="Screenshot 2025-10-07 214905" src="https://github.com/user-attachments/assets/dcc2cc61-3a5f-4fa0-8313-82e1fac778be" />


## Step 3 — Attach Permission Boundary

Select the custom policy DenyUserDeletionBoundary as the permission boundary during user creation.

<img width="1919" height="978" alt="Screenshot 2025-10-07 220002" src="https://github.com/user-attachments/assets/39c17fd4-c4e4-4704-b130-8c7e17425dd3" />

## Step 4 — Privilege Escalation Test

4.1 Attempt to delete another IAM user from the AWS IAM dashboard

Result: Deletion fails with an error:

<img width="1919" height="979" alt="Screenshot 2025-10-07 220536" src="https://github.com/user-attachments/assets/2bace18f-a278-4d1b-9141-4c47c99e19a3" />

"User is not authorized to perform iam:DeleteUser on resource"

Observation: The permission boundary overrides the permissions granted by the identity policy.

## Step 5 — Conclusion

The final effective permissions of the IAM user are the intersection of:

Permissions granted by the identity policy

Permissions allowed by the permission boundary

Even though the identity policy was permissive (IAMFullAccess), the permission boundary restricted deletion actions.

## Key Takeaways

Permission Boundaries enforce least privilege and prevent privilege escalation.

Boundaries restrict permissions rather than grant them.

## Additional visuals

<img width="1405" height="765" alt="Screenshot 2025-10-07 222811" src="https://github.com/user-attachments/assets/c7fbc293-9289-4832-b824-a7b29316ec52" />

<img width="1309" height="770" alt="Screenshot 2025-10-07 222829" src="https://github.com/user-attachments/assets/0a7791f8-0dc7-4f57-8638-e4fe8c041528" />


Final permissions = Identity Policy ∩ Permission Boundary.

⚡ This setup demonstrates how IAM Permission Boundaries can enforce security by restricting permissions granted by identity policies.

## AUTHOR : SUYASH DAHITULE
