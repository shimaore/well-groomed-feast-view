    module.exports = ({emit}) ->
      (doc) ->
        has_attachments = doc._attachments?

        send = (k,v) ->
          emit [null,k...], v
          emit [true,k...], v if has_attachments

        if doc.type? and doc.type is 'voicemail'
          doc.box ?= null
          value =
            type: 'voicemail'
            duration: doc.duration
            caller_id: doc.caller_id
            timestamp: doc.timestamp
            box: doc.box
            message: has_attachments

          send ['voicemail'], value
          send [doc.box,'voicemail'], value
