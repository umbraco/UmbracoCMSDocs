# ContentService Notifications

The ContentService class is the most commonly used type when extending Umbraco using notifications. ContentService implements IContentService. It provides access to operations involving IContent.

## Usage

Example usage of the ContentPublishingNotification:

```C#
using Umbraco.Cms.Core.Events;
using Umbraco.Cms.Core.Services.Notifications;

namespace MySite
{
    public class DontShout : INotificationHandler<ContentPublishingNotification>
    {
        public void Handle(ContentPublishingNotification notification)
        {
            foreach (var node in notification.PublishedEntities)
            {
                if (node.ContentType.Alias.Equals("announcement"))
                {
                    var newsArticleTitle = node.GetValue<string>("title");
                    if (newsArticleTitle.Equals(newsArticleTitle.ToUpper()))
                    {
                        notification.CancelOperation(new EventMessage("Corporate style guideline infringement",
                            "Don't put the announcement title in upper case, no need to shout!",
                            EventMessageType.Error));
                    }
                }
            }
        }
    }
}
```

## Notifications

<table>
  <tr>
    <th>Notification</th>
    <th>Members</th>
    <th>Description</th>
  </tr>

  <tr>
    <td>ContentSavingNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltIContent&gt SavedEntities</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>bool Cancel</li>
      </ul>
    </td>
    <td>
    Published when the IContentService.Save is called in the API.<br />
    NOTE: It can be skipped completely if the parameter "raiseEvents" is set to false during the Save method call (true by default).<br />
    SavedEntities: The collection of IContent objects being saved. <br /><em>NOTE: If the entity is brand new then HasIdentity will equal false.</em>
    </td>
  </tr>

  <tr>
    <td>ContentSavedNotifiaction</td>
    <td>
        <ul>
          <li>IEnumerable&ltIContent&gt SavedEntities</li>
          <li>EventMessages Messages</li>
          <li>IDictionary&ltstring,object&gt State</li>
        </ul>
    </td>
    <td>
    Published when IContentService.Save is called in the API and after data has been persisted.<br />
    NOTE: It can be skipped completely if the parameter "raiseEvents" is set to false during the Save method call (true by default).<br />
    <em>NOTE: <a href="./determining-new-entity">See here on how to determine if the entity is brand new</a></em><br />
    SavedEntities: The saved collection of IContent objects.
    </td>
  </tr>

  <tr>
    <td>ContentPublishingNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltIContent&gt PublishedEntities</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>bool Cancel</li>
      </ul>
    </td>
    <td>
    Published when IContentService.Publishing is called in the API.<br />
    NOTE: It can be skipped completely if the parameter "raiseEvents" is set to false during the Publish method call (true by default).<br />
    <em>NOTE: If the entity is brand new then HasIdentity will equal false.</em><br />
    PublishedEntities: The collection of IContent objects being published.
    </td>
  </tr>

  <tr>
    <td>ContentPublishedNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltIContent&gt PublishedEntities</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
      </ul>
    </td>
    <td>
    Published when IContentService.Publish is called in the API and after data has been published.<br />
    NOTE: It can be skipped completely if the parameter "raiseEvents" is set to false during the Publish method call (true by default).<br />
    <em>NOTE: <a href="./determining-new-entity">See here on how to determine if the entity is brand new</a></em><br />
    PublishedEntities: The published collection of IContent objects.
    </td>
  </tr>

  <tr>
    <td>ContentUnpublishingNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltIContent&gt UnpublishedEntities</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>bool Cancel</li>
      </ul>
    </td>
    <td>
    Published when IContentService.UnPublishing is called in the API.<br />
    UnpublishedEntities: The collection of IContent being unpublished.
    </td>
  </tr>

  <tr>
    <td>ContentUnpublishedNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltIContent&gt UnpublishedEntities</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
      </ul>
    </td>
    <td>
    Published when IContentService.UnPublish is called in the API and after data has been unpublished.<br />
    UnpublishedEntities: The collection of unpublished IContent.
    </td>
  </tr>

  <tr>
    <td>ContentCopyingNotification</td>
    <td>
      <ul>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>bool Cancel</li>
        <li>IContent Original</li>
        <li>IContent Copy</li>
        <li>int ParentId</li>
      </ul>
    </td>
    <td>
    Published when IContentService.Copy is called in the API.<br/>
    The notification is published after a copy object has been created and had its parentId updated and its state has been set to unpublished.<br/>
      <ol>
        <li>Original: Gets the original IContent object.</li>
        <li>Copy: Gets the IContent object being copied.</li>
        <li>ParentId: Gets the Id of the parent of the IContent being copied.</li>
      </ol>
    </td>
  </tr>

  <tr>
    <td>ContentCopiedNotification</td>
    <td>
      <ul>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>IContent Original</li>
        <li>IContent Copy</li>
        <li>int ParentId</li>
        <li>bool RelateToOriginal</li>
      </ul>
    </td>
    <td>
    Published when IContentService.Copy is called in the API.<br/>
    The notification is published after the content object has been copied.<br/>
      <ol>
        <li>Original: Gets the original IContent object.</li>
        <li>Copy: Gets the IContent object being copied.</li>
        <li>ParentId: Gets the Id of the parent of the IContent being copied.</li>
        <li>RelateToOriginal: Boolean indicating whether the copy was related to the original</li>
      </ol>
    </td>
  </tr>

  <tr>
    <td>ContentMovingNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltMoveEventInfo&ltIContent&gt&gt MoveInfoCollection</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>bool Cancel</li>
      </ul>
    </td>
    <td>
    Published when IContentService.Move is called in the API.<br/>
    NOTE: If the target parent is the Recycle bin, this notification is never publused. Try the ContentMovingToRecycleBinNotification instead.<br/>
    MoveInfoCollection will for each moving entity provide:
      <ol>
        <li>Entity: Gets the IContent object being moved</li>
        <li>OriginalPath: The original path the entity is moved from</li>
        <li>NewParentId: Gets the Id of the parent the entity will have after it has been moved</li>
      </ol>
    </td>
  </tr>

  <tr>
    <td>ContentMovedNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltMoveEventInfo&ltIContent&gt&gt MoveInfoCollection</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
      </ul>
    </td>
    <td>
    Published when IContentService.Move is called in the API.
    The notification is published after the content object has been moved.<br/>
    NOTE: If the target parent is the Recycle bin, this notification is never publused. Try the ContentMovedToRecycleBinNotification instead.<br/>
    MoveInfoCollection will for each moving entity provide:
      <ol>
        <li>Entity: Gets the IContent object being moved</li>
        <li>OriginalPath: The original path the entity is moved from</li>
        <li>NewParentId: Gets the Id of the parent the entity will have after it has been moved</li>
      </ol>
    </td>
  </tr>

  <tr>
    <td>ContentMovingToRecycleBinNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltMoveEventInfo&ltIContent&gt&gt MoveInfoCollection</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>bool Cancel</li>
      </ul>
    </td>
    <td>
    Published when ContentService.MoveToRecycleBin is called in the API.<br/>
    MoveInfoCollection will for each moving entity provide:
      <ol>
        <li>Entity: Gets the IContent object being moved</li>
        <li>OriginalPath: The original path the entity is moved from</li>
        <li>NewParentId: Gets the Id of the RecycleBin</li>
      </ol>
    </td>
  </tr>

  <tr>
    <td>ContentMovedToRecycleBinNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltMoveEventInfo&ltIContent&gt&gt MoveInfoCollection</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
      </ul>
    </td>
    <td>
    Published when ContentService.MoveToRecycleBin is called in the API. Is published after the content has been moved to the RecycleBin<br/>
    MoveInfoCollection will for each moving entity provide:
      <ol>
        <li>Entity: Gets the IContent object being moved</li>
        <li>OriginalPath: The original path the entity is moved from</li>
        <li>NewParentId: Gets the Id of the RecycleBin</li>
      </ol>
    </td>
  </tr>

  <tr>
    <td>ContentDeletingNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltIContent&gt DeletedEntities</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>bool Cancel</li>
      </ul>
    </td>
    <td>
    Published when ContentService.DeleteContentOfType, ContentService.Delete, ContentService.EmptyRecycleBin are called in the API.<br/>
    DeletedEntities: Gets the collection of IContent objects being deleted.
    </td>
  </tr>

  <tr>
    <td>ContentDeletedNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltIContent&gt DeletedEntities</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>bool Cancel</li>
      </ul>
    </td>
    <td>Published when ContentService.Delete, ContentService.EmptyRecycleBin are called in the API, and the entity has been deleted.<br/>
    DeletedEntities: Gets the collection of deleted IContent objects.
    </td>
  </tr>

  <tr>
    <td>ContentDeletingVersionsNotification</td>
    <td>
      <ul>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>bool Cancel</li>
        <li>int Id</li>
        <li>int SpecificVersion</li>
        <li>DateTime DateToRetain</li>
        <li>bool DeletePriorVersions</li>
      </ul>
    </td>
    <td>
    Published when ContentService.DeleteVersion, ContentService.DeleteVersions are called in the API.<br/>
      <ol>
        <li>Id: Gets the id of the IContent object being deleted.</li>
        <li>DateToRetain: Gets the latest version date.</li>
        <li>SpecificVersion: Gets the id of the IContent object version being deleted.</li>
        <li>DeletePriorVersions: False by default</li>
      </ol>
    </td>
  </tr>

  <tr>
    <td>ContentDeletedVersionsNotification</td>
    <td>
      <ul>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>int Id</li>
        <li>int SpecificVersion</li>
        <li>DateTime DateToRetain</li>
        <li>bool DeletePriorVersions</li>
      </ul>
    </td>
    <td>Published when ContentService.DeleteVersion, ContentService.DeleteVersions are called in the API, and the version has been deleted.<br/>
      <ol>
        <li>Id: Gets the id of the IContent object being deleted.</li>
        <li>DateToRetain: Gets the latest version date.</li>
        <li>SpecificVersion: Gets the id of the IContent object version being deleted.</li>
        <li>DeletePriorVersions: False by default</li>
      </ol>
    </td>
  </tr>

  <tr>
    <td>ContentRollingBackNotification</td>
    <td>
      <ul>
        <li>IContent Entity</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>bool Cancel</li>
      </ul>
    </td>
    <td>
    Published when ContentService.Rollback is called in the API.<br/>
    Entity: Gets the IContent object being rolled back.
    </td>
  </tr>

  <tr>
    <td>ContentRolledBackNotification</td>
    <td>
      <ul>
        <li>IContent Entity</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
      </ul>
    </td>
    <td>
    Published when ContentService.Rollback is called in the API, after the content has been rolled back.<br/>
    Entity: Gets the IContent object being rolled back.
    </td>
  </tr>

  <tr>
    <td>ContentSendingToPublishNotification</td>
    <td>
      <ul>
        <li>IContent Entity</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>bool Cancel</li>
      </ul>
    </td>
    <td>
    Published when ContentService.SendToPublication is called in the API.<br/>
    Entity: Gets the IContent object being sent to publish.
    </td>
  </tr>

  <tr>
    <td>ContentSentToPublishNotification</td>
    <td>
      <ul>
        <li>IContent Entity</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
      </ul>
    </td>
    <td>
    Published when ContentService.SendToPublication is called in the API, after the entity has been sent to publlication.<br/>
    Entity: Gets the IContent object being sent to publish.
    </td>
  </tr>

  <tr>
    <td>ContentEmptyingRecycleBinNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltIContent&gt DeletedEntities</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
        <li>bool Cancel</li>
      </ul>
    </td>
    <td>
    Raised when ContentService.EmptyRecycleBin is called in the API.<br/>
    DeletedEntities: The collection of IContent objects being deleted.
    </td>
  </tr>

  <tr>
    <td>ContentEmptiedRecycleBinNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltIContent&gt DeletedEntities</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
      </ul>
    </td>
    <td>
    Raised when ContentService.EmptyRecycleBin is called in the API, after the RecycleBin has been emptied.<br/>
    DeletedEntities: The collection of deleted IContent object.
    </td>
  </tr>

  <tr>
    <td>ContentSavedBlueprintNotification</td>
    <td>
      <ul>
        <li>IContent SavedBlueprint</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
      </ul>
    </td>
    <td>
    Raised when ContentService.SavedBlueprint is called in the API.<br/>
    SavedBlueprint: Gets the saved blueprint IContent object
    </td>
  </tr>

  <tr>
    <td>ContentDeletedBlueprintNotification</td>
    <td>
      <ul>
        <li>IEnumerable&ltIContent&gt DeletedBlueprints</li>
        <li>EventMessages Messages</li>
        <li>IDictionary&ltstring,object&gt State</li>
      </ul>
    </td>
    <td>
    Raised when ContentService.DeletedBlueprint is called in the API.<br/>
    DeletedBlueprints: The collection of deleted blueprint IContent
    </td>
  </tr>

</table>