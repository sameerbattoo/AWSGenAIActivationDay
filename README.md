# AWSGenAIActivationDay
Repo for Strands Agent &amp; deployment to AgentCore
This repo contains various notebooks to be run locally using VS Code or an equivalent IDE.

### Prerequisites

- An [AWS account](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fportal.aws.amazon.com%2Fbilling%2Fsignup%2Fresume&client_id=signup) with credentials configured (`aws configure`)
- [Python 3.10](https://www.python.org/downloads/) or later
- [Docker](https://www.docker.com/) or [Finch](https://runfinch.com/) installed and running - only for local development
- Model Access: Anthropic Claude 4.0 enabled in [Amazon Bedrock console](https://docs.aws.amazon.com/bedrock/latest/userguide/model-access-modify.html)
- [VS Code](https://code.visualstudio.com/) or equivalent as a IDE to the run the notebooks locally on your laptop.
- AWS Permissions:
    1) Create a **IAM Policy** named **"_GenAIActivationDay_policy"**
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "s3:GetObject",
                    "s3:PutObject",
                    "s3:DeleteObject",
                    "s3:ListBucket",
                    "s3:DeleteBucket",
                    "s3:PutEncryptionConfiguration",
                    "s3:PutLifecycleConfiguration",
                    "s3:PutBucketVersioning",
                    "s3:PutBucketPublicAccessBlock",
                    "s3:DeleteBucketPolicy",
                    "s3:PutBucketPolicy"
                ],
                "Resource": "arn:aws:s3:::*"
            },
            {
                "Effect": "Allow",
                "Action": "s3:CreateBucket",
                "Resource": [
                    "arn:aws:s3:::*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": [
                    "iam:GetRole",
                    "iam:GetRolePolicy",
                    "iam:PutRolePolicy",
                    "iam:CreateRole",
                    "iam:DeleteRole",
                    "iam:CreatePolicy",
                    "iam:AttachRolePolicy",
                    "iam:CreateServiceLinkedRole",
                    "iam:DeleteRole",
                    "iam:DeletePolicy",
                    "iam:PassRole",
                    "iam:GetPolicy",
                    "iam:DetachRolePolicy",
                    "iam:DeleteRolePolicy",
                    "iam:ListAttachedRolePolicies",
                    "iam:TagRole"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "bedrock:CreateKnowledgeBase",
                    "bedrock:UpdateKnowledgeBase",
                    "bedrock:DeleteKnowledgeBase",
                    "bedrock:GetKnowledgeBase",
                    "bedrock:ListKnowledgeBases",
                    "bedrock:Retrieve",
                    "bedrock:RetrieveAndGenerate",
                    "bedrock:ListDataSources",
                    "bedrock:ListIngestionJobs",
                    "bedrock:GetDataSource",
                    "bedrock:StartIngestionJob",
                    "bedrock:InvokeModel",
                    "bedrock:InvokeModelWithResponseStream",
                    "bedrock:CreateDataSource",
                    "bedrock:GetIngestionJob",
                    "bedrock:DeleteDataSource"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "bedrock:CreateAgent",
                    "bedrock:AssociateAgentKnowledgeBase",
                    "bedrock:CreateAgentActionGroup",
                    "bedrock:CreateAgentAlias",
                    "bedrock:DeleteAgent",
                    "bedrock:DeleteAgentMemory",
                    "bedrock:DeleteAgentActionGroup",
                    "bedrock:DeleteAgentVersion",
                    "bedrock:DeleteAgentAlias",
                    "bedrock:PrepareAgent",
                    "bedrock:DisassociateAgentKnowledgeBase",
                    "bedrock:UpdateAgentAlias",
                    "bedrock:UpdateAgentActionGroup",
                    "bedrock:UpdateAgent",
                    "bedrock:UpdateAgentKnowledgeBase",
                    "bedrock:TagResource",
                    "bedrock:UntagResource",
                    "bedrock:GetAgent",
                    "bedrock:GetAgentKnowledgeBase",
                    "bedrock:GetAgentAlias",
                    "bedrock:GetAgentVersion",
                    "bedrock:GetAgentActionGroup",
                    "bedrock:GetAgentMemory",
                    "bedrock:InvokeInlineAgent",
                    "bedrock:InvokeAgent",
                    "bedrock:ListAgentActionGroups",
                    "bedrock:ListAgentAliases",
                    "bedrock:ListAgentKnowledgeBases",
                    "bedrock:ListAgentVersions",
                    "bedrock:ListAgents",
                    "bedrock:AssociateAgentCollaborator",
                    "bedrock:DisassociateAgentCollaborator",
                    "bedrock:UpdateAgentCollaborator",
                    "bedrock:GetAgentCollaborator",
                    "bedrock:ListAgentCollaborators"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "bedrock:GetGuardrail",
                    "bedrock:ApplyGuardrail",
                    "bedrock:CreateGuardrail",
                    "bedrock:DeleteGuardrail"
                ],
                "Resource": "arn:aws:bedrock:*:*:guardrail/*"
            },
            {
                "Effect": "Deny",
                "Action": [
                    "bedrock:CreateModelCustomizationJob",
                    "bedrock:StopModelCustomizationJob",
                    "bedrock:GetModelCustomizationJob",
                    "bedrock:ListModelCustomizationJobs"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "aoss:BatchGetLifecyclePolicy",
                    "aoss:GetAccessPolicy",
                    "aoss:CreateAccessPolicy",
                    "aoss:UpdateSecurityConfig",
                    "aoss:UpdateLifecyclePolicy",
                    "aoss:UpdateSecurityPolicy",
                    "aoss:CreateLifecyclePolicy",
                    "aoss:ListAccessPolicies",
                    "aoss:ListSecurityPolicies",
                    "aoss:UpdateAccessPolicy",
                    "aoss:DeleteSecurityPolicy",
                    "aoss:UntagResource",
                    "aoss:GetSecurityPolicy",
                    "aoss:ListTagsForResource",
                    "aoss:BatchGetCollection",
                    "aoss:ListLifecyclePolicies",
                    "aoss:ListSecurityConfigs",
                    "aoss:DeleteLifecyclePolicy",
                    "aoss:CreateSecurityConfig",
                    "aoss:CreateSecurityPolicy",
                    "aoss:TagResource",
                    "aoss:BatchGetVpcEndpoint",
                    "aoss:GetPoliciesStats",
                    "aoss:ListVpcEndpoints",
                    "aoss:UpdateAccountSettings",
                    "aoss:GetAccountSettings",
                    "aoss:GetSecurityConfig",
                    "aoss:BatchGetEffectiveLifecyclePolicy",
                    "aoss:DeleteSecurityConfig",
                    "aoss:ListCollections",
                    "aoss:DeleteAccessPolicy",
                    "aoss:CreateCollection",
                    "aoss:UpdateCollection",
                    "aoss:DeleteCollection"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": "aoss:APIAccessAll",
                "Resource": "arn:aws:aoss:*:*:collection/*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "lambda:AddPermission",
                    "lambda:CreateFunction",
                    "lambda:DeleteFunction",
                    "lambda:GetFunction",
                    "lambda:InvokeFunction",
                    "lambda:CreateEventSourceMapping",
                    "lambda:GetEventSourceMapping"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "dynamodb:DescribeTable",
                    "dynamodb:CreateTable",
                    "dynamodb:DeleteTable",
                    "dynamodb:ListTables",
                    "dynamodb:GetItem",
                    "dynamodb:Query",
                    "dynamodb:PutItem",
                    "dynamodb:DeleteItem",
                    "dynamodb:UpdateItem",
                    "dynamodb:Scan"
                ],
                "Resource": "arn:aws:dynamodb:*:*:table/*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "cloudformation:CreateChangeSet",
                    "cloudformation:ExecuteChangeSet",
                    "cloudformation:DeleteChangeSet",
                    "cloudformation:DeleteStack",
                    "cloudformation:ListStacks",
                    "cloudformation:UpdateStack",
                    "cloudformation:CreateStack"
                ],
                "Resource": "arn:aws:cloudformation:*:*:stack/*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "ssm:PutParameter",
                    "ssm:GetParameters",
                    "ssm:GetParameter",
                    "ssm:DeleteParameter",
                    "sns:*"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "ecr:PutLifecyclePolicy",
                    "ecr:SetRepositoryPolicy",
                    "ecr:InitiateLayerUpload",
                    "ecr:UploadLayerPart",
                    "ecr:CompleteLayerUpload",
                    "ecr:PutImage",
                    "ecr:DeleteRepository"
                ],
                "Resource": "arn:aws:ecr:*:*:repository/*"
            },
            {
                "Effect": "Allow",
                "Action": "sns:*",
                "Resource": "*"
            }
        ]
    }
    ```

    2) Create a **IAM Role** named **"_GenAIActivationDay_role"**
    with the following **TRUST Relastionship**
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Principal": {
                    "Service": "sagemaker.amazonaws.com"
                },
                "Action": "sts:AssumeRole"
            },
            {
                "Sid": "AssumeRolePolicy",
                "Effect": "Allow",
                "Principal": {
                    "Service": "bedrock-agentcore.amazonaws.com"
                },
                "Action": "sts:AssumeRole"
            }
        ]
    }
    ```

        And, the following **Permissions policies**
        
        - _GenAIActivationDay_policy
        - AWSCloudFormationReadOnlyAccess
        - AmazonSageMakerFullAccess

    3) Add the newly created policy  **"GenAIActivationDay_policy"** to your IAM user configured locally (used by VS Code)
