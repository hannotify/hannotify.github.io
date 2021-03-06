
{% capture nowUnix %}{{'now' | date: '%s'}}{% endcapture %}
{% assign pastEvents = "" | split: "" %}

{% for event in site.data.events %}
    {% capture eventTime %}{{event.dateStart | date: '%s'}}{% endcapture %}

    {% if nowUnix > eventTime %}
        {% assign pastEvents = pastEvents | push: event %}
    {% endif %}
{% endfor %}

# [](#upcoming-events)Past events ({{ pastEvents.size }})

{% for event in pastEvents %}
## [](#{{event.id}})[{{event.name}} {{event.year}}]({{event.url}})

<table>
    <tr>
        <td><strong>Location</strong></td>
        {% if event.city %}
            <td>{{event.city}}, {{event.country.name}} <img src="images/flags/{{event.country.flag}}.gif"/></td>
        {% else %}
            <td>Online</td>
        {% endif %}
    </tr>
    <tr>
        <td><strong>Date</strong></td>
        {% if event.dateEnd %}
            <td>{{ event.dateStart | date: "%B %e, %Y" }} - {{ event.dateEnd | date: "%B %e, %Y" }}</td>
        {% else %}
            <td >{{ event.dateStart | date: "%B %e, %Y" }}</td>
        {% endif %}
    </tr>
    {% for appearance in event.appearances %}{% assign talk = site.data.talks | where: "id", appearance.id | first %}    
        <tr>
            <td><strong>Talk</strong></td>
            <td><a href="talks#{{talk.id}}">{{talk.title}}</a></td>
        </tr>
        {% if talk.cospeakers != null and talk.cospeakers.size > 0 %}
            {% for cospeaker in talk.cospeakers %}
            <tr>
                <td><strong>Co-speaker</strong></td>
                <td><a href="{{cospeaker.twitterUrl}}" target="_blank">{{cospeaker.name}}</a></td>
            </tr>
            {% endfor %}
        {% endif %}
        {% if appearance.sessionPageUrl or appearance.videoUrl or appearance.slidesUrl or appearance.githubUrl %}
            <tr>
                <td>&nbsp;</td>
                <td>
                    {% if appearance.sessionPageUrl %}
                        <a target="_blank" href="{{appearance.sessionPageUrl}}"><code>session page</code></a>
                    {% endif %}
                    {% if appearance.videoUrl %}
                        <a target="_blank" href="{{appearance.videoUrl}}"><code>video</code></a>
                    {% endif %}
                    {% if appearance.slidesUrl %}
                        <a target="_blank" href="{{appearance.slidesUrl}}"><code>slides</code></a>
                    {% endif %}
                    {% if appearance.githubUrl %}
                        <a target="_blank" href="{{appearance.githubUrl}}"><code>github</code></a>
                    {% endif %}
                </td>
            </tr>  
        {% endif %}  
    {% endfor %}
</table>
{% endfor %}
