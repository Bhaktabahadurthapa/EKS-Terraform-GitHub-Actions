{
    "Version": "2012-10-17",
    "Id": "Policy1724116378123",
    "Statement": [
        {
            "Sid": "Stmt1724116373206",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::058264297091:user/Bhakta-access"
            },
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::my-aws-bucket-project-1",
                "arn:aws:s3:::my-aws-bucket-project-1/*"
            ]
        }
    ]
}
