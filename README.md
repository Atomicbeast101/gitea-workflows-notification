# Gitea Workflows - Notification System

Workflow template for Pushover notifications for success/failed DevOps pipeline runs in my Homelab.

## Usage

```yaml
  report:
    name: Report
    continue-on-error: true
    needs: 
      - release
    steps:
      - name: Get Meta
        id: meta
        run: |
          echo REPO_NAME=$(echo ${GITHUB_REPOSITORY} | awk -F"/" '{print $2}') >> $GITHUB_OUTPUT
          echo DATE_RELEASE=$(echo "`date +'%Y.%m.%d'`") >> $GITHUB_OUTPUT

      - name: Notify User
        uses: https://${{ vars.LOCAL_ACR_URL }}/${{ vars.LOCAL_ACR_USERNAME }}/gitea-workflows-notification@main
        with:
          success: needs.release.result == 'success'
          repo_name: ${{ steps.meta.outputs.REPO_NAME }}
          image_tag_ending: ${{ env.IMAGE_TAG_ENDING }}
          date_release: ${{ steps.meta.outputs.DATE_RELEASE }}
          pushover_user: ${{ secrets.PUSHOVER_USER }}
          pushover_token: ${{ secrets.PUSHOVER_TOKEN }}
```
