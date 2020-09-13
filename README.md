# AKS 구축을 위한 사전 작업
## 로컬 머신에 Azure cli 설치 후 Azure에 로그인

### Mac
#### 설치
`
brew update && brew install azure-cli
`
#### 로그인
`
az login
`
#### 구독 확인
`
az account list --output table
`
#### 하나의 구독을 기본 값으로 지정
`
az account set -s "ACCOUNT_ID"
`
